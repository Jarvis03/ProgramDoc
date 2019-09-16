### Freetype
#### PC
freetype-2.4.10.tar.bz2 ������linux��Ȼ���ѹ `tar xjf freetype-2.4.10.tar.bz2` ����Ϊ `mv freetype-2.4.10 freetype-2.4.10_pc` ����Ŀ¼ִ��`./config -> make -> sudo make install` ��װ�� `/usr/local.lib/`


gcc -o example1 example1.c �����ļ�
��������Ҳ���ͷ�ļ�����Ҫ����ʱָ��
`gcc -o example1 example1.c -I /usr/local/include/freetype2` ������ʾ����δ���壬��Ҫ�����`gcc -o example1 example1.c -I /usr/local/include/freetype2 -lfreetype -lm(math)`
##### �������
��ѹ`tra xjf freetype-2.4.10.tar.bz2`


./configure --host=arm-linux
make
make DESTDIR=$PWD/tmp install

���������ͷ�ļ�Ӧ�÷��룺
`/usr/local/arm/4.3.2/arm-none-linux-gnueabi/libc/usr/include` 

�ҵ�
`/work/tools/gcc-3.4.5-glibc-2.3.6/arm-linux/include`

��������Ŀ��ļ�Ӧ�÷��룺
`/usr/local/arm/4.3.2/arm-none-linux-gnueabi/libc/armv4t/lib`

�ҵ� `/work/tools/gcc-3.4.5-glibc-2.3.6/arm-linux/lib`


��tmp/usr/local/include/*  ���Ƶ� /usr/local/arm/4.3.2/arm-none-linux-gnueabi/libc/usr/include
cp * /usr/local/arm/4.3.2/arm-none-linux-gnueabi/libc/usr/include -rf
cd /usr/local/arm/4.3.2/arm-none-linux-gnueabi/libc/usr/include
mv freetype2/freetype .

arm-linux-gcc -finput-charset=GBK -o example1 example1.c  -lfreetype -lm
arm-linux-gcc -finput-charset=GBK -o show_font show_font.c  -lfreetype -lm


freetype/config/ftheader.h
freetype2/freetype/config/ftheader.h 



arm-linux-gcc -finput-charset=GBK -fexec-charset=GBK -o show_font show_font.c -lfreetype -lm 
`arm-linux-gcc -o example1 example1.c -I /usr/local/include/freetype2 -lfreetype -lm(math)`