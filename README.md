# sina_code_distinguish
python 新浪模拟登陆验证码识别

验证码识别的一般思路为：

    1、图片降噪

    2、图片切割

    3、图像文本输出
    
1.图片降噪，文件show.gif是http://weibo.cn/search/?tf=5_012&vt=4保存的最原始的验证码，降噪主要包括

    1.1去除干扰线：干扰线的像素值(4,2,4)都一样，且与字符的像素值不一样，所以只要将干扰线的像素值(4,2,4)转为白色(255,255,255)就行

    1.2去掉离散干扰点：参考过http://www.programgo.com/article/84313451055/的方法，但是这里的字符是空心的，这种方法会把字符的像素点去掉，所以没有采用。因为验证码干扰点不是很多，所以不会造成很大影响。

    1.3二值化：

2.图片切割，sina验证码的切割是本验证码的难点，因为不像其他的验证码，每一个字符都是固定长度，只要设定长度，就可以将每个字符切割出来，sina的验证码，每个字符所占的长度是不一致的，这样就不能用固定长度进行切割，所以采用了另外一种切割方式：

如文件show.png所示，将验证码（应该是去噪之后的验证码）按列网格化，统计每个网格点内的像素点：
[  0.   0.   4.  10.   3.   7.   7.   4.   5.   6.   7.   6.   0.   0.   0…0.   0.   5.   5.   6.   2.   5.   9.   4.   5.   8.   3.   2.   0.   0. 0.   0.   0.   0.   8.   7.   2.   9.   6.   7.   8.   6.   7.    7.   2. 0.   0.   1.   5.   5.   7.  11.  10.   9.   5.   3.   3.   1.   0.   0. 0.   0.   0.   0.   0.   0.   0.   0.   0.   0.   0.   0.   0.   0.   0. 0.   0.   0.   0.   0.   0.   0.   0.   0.   0.   0.   0.   0.   0.   0. 0.   0.   0.   0.   0.   0.   0.   0.   0.   0.]

虽然每个字符所占的网格点一样，但是每个字符所占的像素点至少有一个，如：
[0.   0.   4.  10.   3.   7.   7.   4.   5.   6.   7.   6.   0.   0],
即为一个字符所占的网格点，同时，字符与字符之间的为空白像素点为0，这样就可以把验证码切割出来。

3图像文本输出
   3.1建立字符库：从http://weibo.cn/search/?tf=5_012&vt=4上保存100张验证码，然后将其分割，建立字符库model。
   3.2建立字符字典：人工识别字符库里面的字符，并保存到model.txt,如:
    h.gif h
    y.gif y
    3-1.gif 3
    5_1_new.gif 3
    6_1_new.gif c
    6_2_new.gif j
    6_3_new.gif m
    7_1_new.gif w
    7_2_new.gif l
    8_1_new.gif n
    8_2_new.gif x
  3.2互相关，识别字符：



