
#### LCD��������

##### ����
###### app:  
     open("/dev/fb0", ...)   ���豸��: 29, ���豸��: 0

##### kernel:
         fb_open
         	int fbidx = iminor(inode);
         	struct fb_info *info = = registered_fb[0];

###### app:  
    read()

###### kernel:
		fb_read
			int fbidx = iminor(inode);
			struct fb_info *info = registered_fb[fbidx];
			if (info->fbops->fb_read)
				return info->fbops->fb_read(info, buf, count, ppos);
         	
			src = (u32 __iomem *) (info->screen_base + p);
			dst = buffer;
			*dst++ = fb_readl(src++);
			copy_to_user(buf, buffer, c)         	

##### ��  1. registered_fb�����ﱻ���ã�
    ��1. register_framebuffer

##### ��ôдLCD��������
    1. ����һ��fb_info�ṹ��: framebuffer_alloc
    2. ����
    3. ע��: register_framebuffer
    4. Ӳ����صĲ���

##### ���ԣ�
      1. make menuconfigȥ��ԭ������������
        -> Device Drivers
          -> Graphics support
        <M> S3C2410 LCD framebuffer support

    2. make uImage
       cp arch/arm/boot/uImage  /work/nfs_root/uImage_nolcd
       
       make modules 
       cp drives/video/cfb*.ko  /work/nfs_root/first_fs

    3. ʹ���µ�uImage����������:
       root
       nfs 30000000 192.168.3.108:/work/nfs_root/uImage_nolcd
       bootm 30000000

    4. 
      insmod cfbcopyarea.ko 
      insmod cfbfillrect.ko 
      insmod cfbimgblt.ko 
      insmod lcd.ko

##### echo hello > /dev/tty1  // ������LCD�Ͽ���hello
##### cat lcd.ko > /dev/fb0   // ����

###### 5. �޸� /etc/inittab
    tty1::askfirst:-/bin/sh
    �����ں�����������

    insmod cfbcopyarea.ko 
    insmod cfbfillrect.ko 
    insmod cfbimgblt.ko 
    insmod lcd.ko
    insmod buttons.ko
    
    echo hello > /dev/tty1
    cat lcd.ko > /dev/fb0


