# Cannot delete or update a parent row: a foreign key constraint fails

### 배경 지식 요약
회사에서는 Django 에서 제공하는 ORM 기능을 사용하고 있습니다. ORM QueryEngine이 Model Object Code를 DDL로 변환해 DB Schema를 갱신합니다. 회사 컨벤션상 FK는 CASCADE 기능을 사용하지 않습니다.
_
### 문제 발생 형환
테스트 데이터의 누락으로 인해 데이터 이슈가 발생하게 되었습니다. Production에서 User Flow로 Process를 진행 한다면 발생하지 않을 이슈지만, 개발환경에서 이것 저것 테스트 하기 위해 쌓인 데이터(rows) 덕분에? 데이터 이슈가 생겼습니다. 하지만 저희는 모든 테이블에 `CASCADE` OFF 되어있습니다.
> MySQL의 경우 CREATE TABLE시 FK에 제약조건 선언을 하지 않을시 데이터 정합성(Consistency)을 위하여 기본값으로 `ON DELETE RESTRICT` 와 `ON UPDATE RESTRICT`적용되어 있습니다.

_
### 해결방법
ORM 상에서 지원하는 간단한 방법이 없을까 찾았지만 Raw Query 작성보다 쉬워보이지 않았습니다. 결국은 Query로 해결하기로 합니다.
_

1. FK가 걸려있는 테이블 Schema 확인. 외래키 제약 조건 확인. 아래의 경우 DEFAULT.

	```sql
	SHOW CREATE TABLE city;
	```

	```sql
	CREATE TABLE `city` (
	  `ID` int(11) NOT NULL AUTO_INCREMENT,
	  `Name` char(35) NOT NULL DEFAULT '',
	  `CountryCode` char(3) NOT NULL DEFAULT '',
	  `District` char(20) NOT NULL DEFAULT '',
	  `Population` int(11) NOT NULL DEFAULT '0',
	  PRIMARY KEY (`ID`),
	  KEY `CountryCode` (`CountryCode`),
	  CONSTRAINT `city_ibfk_1` FOREIGN KEY (`CountryCode`) REFERENCES `country` (`Code`)
	) ENGINE=InnoDB AUTO_INCREMENT=4080 DEFAULT CHARSET=latin1
	
	```

2. ON DELETE REASTRICT` 제약조건 확인. DELETE query 실패 확인.

	```sql
	DELETE FROM country WHERE `Code` = (SELECT CountryCode FROM city LIMIT 1)
	```
	
	```
	Cannot delete or update a parent row: a foreign key constraint fails (`world`.`city`, CONSTRAINT `city_ibfk_1` FOREIGN KEY (`CountryCode`) REFERENCES `country` (`Code`))
	```
_

3. country table을 참조하는 table의 FK 삭제후, `CASCADE` 적용

	```
	ALTER TABLE `city` DROP FOREIGN KEY `city_ibfk_1`;
	```
	```
	ALTER TABLE `city` ADD FOREIGN KEY (`CountryCode`) REFERENCES `country` (`Code`)
	ON DELETE CASCADE ON UPDATE CASCADE;
	```
	```
	ALTER TABLE `countrylanguage` DROP FOREIGN KEY `countryLanguage_ibfk_1`;
	```
	```
	ALTER TABLE `countrylanguage` ADD FOREIGN KEY (`CountryCode`) REFERENCES `country` (`Code`)
	ON DELETE CASCADE ON UPDATE CASCADE;
	```
_

4. `CASCADE` 적용 확인. 2번 과정에서 실패했던 query 재실행 후 성공확인. 성공했다면 `CASCADE` 적용 성공.
	
	```sql
	DELETE FROM country WHERE `Code` = (SELECT CountryCode FROM city LIMIT 1);
	```
	
5. UPDATE query 실행 후 데이터 갱신을 확인해보자. 

	```sql
	SELECT * FROM city WHERE CountryCode = "AFG";
	1	Kabul	AFG	Kabol	1780000
	2	Qandahar	AFG	Qandahar	237500
	3	Herat	AFG	Herat	186800
	4	Mazar-e-Sharif	AFG	Balkh	127800
	
	UPDATE `country` SET `Code`="QQQ" WHERE `Code`="AFG";
	
	SELECT * FROM city WHERE CountryCode = "AFG";
	return 0rows.
	
	SELECT * FROM city WHERE CountryCode = "QQQ";
	1	Kabul	QQQ	Kabol	1780000
	2	Qandahar	QQQ	Qandahar	237500
	3	Herat	QQQ	Herat	186800
	4	Mazar-e-Sharif	QQQ	Balkh	127800
	```
	
	결과를 확인해보면, city table에 참조관계가 있는 tables들의 rows도 함께 갱신되고, `consistency`가 보장되는것을 확인할 수 있다.
	