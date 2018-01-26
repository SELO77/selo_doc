Tags: decorator, closer, python


# Understanding decorator and closer on the Python


```python
def deco1(deco_args, ):
	print('deco1 s')
	def deco1_inner(func):
		print('deco1_inner s')

		def wrapper(*args):
			print('deco1.wrapper')
			print('deco_args: ', deco_args)
			return func(*args)

		print('deco1_inner e')
		return wrapper

	print('deco1 e')
	return deco1_inner


def deco2(func):
	print('deco2 s')

	def wrapper(*args):
		print('deco2.wrapper')
		return func(*args)

	print('deco2 e')
	return wrapper


@deco2
@deco1(['name', 'age', 'sex'])
def origin_func(*args):
	print('origin_func s')
	print('origin_func args:', args)
	print('origin_func e')


if __name__ == '__main__':
	origin_func(*['hello', 'selo'])

```