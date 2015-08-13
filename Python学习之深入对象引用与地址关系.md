#Python学习之深入引用对象与地址关系

####Python示例
	>>> L1 = [1,2]
	>>> L1
	[1, 2]
	>>> L2 = [3,4]
	>>> L1.append(L2)
	>>> L1
	[1, 2, [3, 4]]
	>>> L2.pop()
	4
	>>> L1
	[1, 2, [3]]
#####通过观察上面的例子,我们可以得出一个结论:当L1添加L2时,只是将L2引用对象指向的地址添加进来,而L2改变时,此时的L1也随之改变.
 
####Java示例
	List a = new ArrayList(); 
	a.add(0,"1");
	a.add(1,"2");
	List b = new ArrayList(); 
	b.add(0,"3");
	b.add(1,"4");
	a.add(b);
	b.remove(0);
	System.out.println(a+"   " +b);  //[1,2,[4]]    [4]  同样如此