## File Manipulation & Management

#### cp, mv, rm, alias, unalias

#### glob (globbing)
- Globbing pathnames
- find pathnames matching a pattern, free memory from glob();
- bash 中用于实现文件名通配
- Wildcards
	- *
		- 任意长度的任意字符
	- ?
		- 任意单个字符
	- []
		- 0-9
		- a-z (case insensitive), A-Z (upper case, case sensitive)
		- ^ 取反, 匹配指定范围外的任意单个字符
		- 专用字符集合 (parts of them, often used ones)
			- [:digit:]: any number, equals to 0-9
			- [:lower:]: any lower cases
			- [:upper:]: any upper cases
			- [:alnum:]: any numbers or letters
			- [:alpha:]: any letters
