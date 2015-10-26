**（一）：写在前面**

在这一小节当中，我主要是实现了对frame buffer的操作编程，实现了将内存中的地址映射到逻辑地址空间，然后对其内存进行操作，包括在屏幕上画点，画线，画四边形，填充四边形等．然后，再将数据映射到内存中进行显示．这里的操作比较简单，只要实现一个画点的操作，就能以画点为基础，实现各种操作．

**（二）：画点的实现**

首先，我们在上一个小节中已经将内存中的地址映射到相应的逻辑地址内存空间．就是使用mmap()函数．有关与mmap()函数的参数，在网上一搜即可．

当获取到相应的地址之后，我们就是根据相应地址的偏移量来对内存设置RGB和透明度的值．

```
	uint32_t offset;
	uint8_t color[4];
	color[0] = b;
	color[1] = g;
	color[2] = r;
	color[3] = 0x0; //透明度
	
    //偏移量
	offset = p.y * pFbdev->fb_fix.line_length + 4 * p.x;
    
```

颜色设置完成之后，我们需要将相应的值写入到内存当中，在这里，我们重新实现了一个函数，那就是memcpy()函数．

```
//映射到内存
void fb_memcpy(void *addr,void *color,size_t len)
{
	memcpy(addr,color,len);
}

```

最后，我们使用该函数，将相应的内容映射到内存中．

```
//将操作映射到内存中
	fb_memcpy((void*)pFbdev->fb_mem + pFbdev->fb_mem_offset + offset,color,4);
    
```

**（四）：编译运行方法**

在此次源码文件中，我添加了一个Makefile文件，所以，如果要编译文件的话，只需使用make命令，就能将相应的test可执行文件编译出来．编译完成之后，我们使用ctrl+alt+f1进入到命令行界面，然后，进入到该目录运行．就可看出相应的运行结果．

**（五）：运行结果展示**

下面是我使用手机拍摄的一张我的运行结果的照片．还是很漂亮的．

**（六）：后期规划**

后面我打算能不能在命令行界面上实现一个图形化界面的库．这个难度其实是很大的，不过我觉着还是很有意思的．慢慢来吧，一个函数一个函数的写，一个函数一个函数的测试，脚踏实地．

**（七）：写在后面**

昨天已经过去，明天还未来临，我们能做到只有活在当下．