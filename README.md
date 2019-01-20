# basic-ways-of-encryption-decryption
1.	对于mfc的编辑对话框内只支持CString类型字符数据传输，刚开始时对于类型的转换有很多疑惑。
在解决类型转换的过程中发现如果用：
LPCTSTR str =  in_str;
out_str_ary.Add(LPCTSTR str);
会导致在对数据处理的同时，将原始数据同时改变，导致加解密时出现逻辑错误。
然后发现一种转换类型的方式：
chartemp = strtemp.GetBuffer(strtemp.GetLength());
strtemp.ReleaseBuffer();
此方法运行成功。
同样，char*转为CString时的语句为：
CString strtemp(chartemp);
target = strtemp;
直接将chartemp转成strtemp会出现错误。

2.	对于每个加解密方式，都有两个类，一个用来处理加解密功能，另一个用来弹出结果。

3.	VS2017版本对MFC维护较差，在关联变量时不会自动为你添加关联语句，需要手动加入语句：DDX_Text(pDX, IDC_key, key);
同样，在为控件改变ID值后，引用会出现无法识别的错误，此时需要添加头文件Resource.h,再重新运行一下程序，若还是无法识别，关掉VS再打开就可以了。

4.	在调试程序时出现了堆栈溢出的问题，出现这个错误大多是因为在需要为变量分配空间时发现为NULL，系统无法合理分配空间，在对程序进行分步调试时发现对话框的变量并没有成功传递给函数，导致数据为空。

5.	除了LSB加解密，其他的方式大多大同小异，LSB加解密方式的不同之处在于需要在外部引入外部图片和文本文件以及需要构建结构体。
在引入外部图片时发现每次需要将文件路径名打入非常麻烦，于是引用了可以视图选择文件的对话框类，使选择文件更为方便。
在构建结构体时发现MFC自带了这些结构体，然而却不能调用，后发现系统自带的结构体为14位，而自己构建的是14位，无法兼容。
解决方法是在构造结构体时加入语句：
#pragma   pack(push)  
#pragma   pack(1)  
struct 结构体
{
	 
};                           
#pragma     pack(pop)   

6.	在新建CString类型变量时最好为其附上初始值，保证显示正确。
