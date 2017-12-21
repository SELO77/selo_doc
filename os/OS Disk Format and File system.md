Tags: OS, file system


# OS Disk Format and File system

2018년 새해 맞이 준비를 위해 개인 맥 포맷을 진행했고, MAC 파일시스템에 대한 이해를 위해 정리를 해보려 한다.


* APFS : Appple 파일시스템, APFS는 SSD(솔리드 스테이트 드라이브) 및 기타 모든 플래시 저장 장치의 기본 파일 시스템으로, Mac OS 확장(HFS+)을 대체합니다.
* MAC OS Extended (Journaled, Encrypted, Case-sensitive)
	* MAC OS 파일시스템, 저널링 기능 추가. 	
	* 저널링 파일 시스템 : 주파일 시스템에 변경사항을 반영하기 전에, 저널안에 생성되는 변경사항을 추적하는 파일시스템. 시스템 전원문제 등 시스템 다운으로 부터 파일의 손상 가능성이 낮다.
		* 원리: 파일과 디렉토리의 변경은 여러 개의 쓰기 오퍼레이션으로 실행하고, 이러한 이유로 전원공급이나 시스템 오류와 같은 인터럽트로 인한 데이터의 손실 가능성을 낮출 수 있습니다. 정리하자면 작동을 작은단위로 나누어 실행함으로서 중간에 인터럽트가 일어나도 멈춘지점을 추적할 수 있습니다.
		* `Journal(일기)`: 변경사항을 기록하는 특별 구역
		* 정리하자면 저널 파일시스템은 변경사항을 기록하는 특별구역(저널)을 할당하고, 문제가 발생할 경우, 저널을 읽고, 읽은 내용을 되돌리는 것으로 복구가 이루어진다.
		* 필요한 이유: 유닉스 파일 시스템에서 파일을 지우는 것은 크게 두 단계다. 첫번째 삭제 파일의 디렉터리 엔트리를 삭제한다. 두번째 파일이 차지하던 공간과 아이노드를 해제하여 free space map에 표기한다. 해당과정 중간에 시스템 문제가 발생할 경우 다음과 같은 이슈가 생긴다. free space map이 갱신되지 않아 `스토리지 누수`가 발생한다. 또는 지워지지 않은 파일 영역이 쓰기 가능상태(free)로 표시되어 다른 내용으로 덮어쓰여 질 수 있다.
	* Encrypted File System : 저장 데이터 암호화
	* Case-sensitivity : trate upper case and lower case as distinct

		
		

## Reference
* [저널링 파일 시스템](https://ko.wikipedia.org/wiki/%EC%A0%80%EB%84%90%EB%A7%81_%ED%8C%8C%EC%9D%BC_%EC%8B%9C%EC%8A%A4%ED%85%9C)
* [Mac용 디스크 포맷 시 APFS와 Mac OS 확장 중에서 선택하는 방법](https://support.apple.com/ko-kr/HT208033)

