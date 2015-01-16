#Huffman编码压缩算法

最近想写一个C++的简易压缩工具，用的算法是Huffman算法，再次先复习一下这个算法的思想。

我们直接来看实例，如果我们需要来压缩下面的字符串：

**"beep boop beer!"**

首先，我们先计算出每个字符串出现的次数，我们的到下面一张表：

	|字符|次数|
	|b  |3  |
	|e  |4  |
	|p  |2  |
	|"  |2  |
	|o  |2  |
	|r  |1  |
	|!  |1  |
然后，我们把这些东西放大一个priotriy Queue的中(用出现的次数当做priority)，我们看到，priority Queue是以priority排序的一个数组，如果ptiority一样，会使用出现的次数排序，下面就是我们得到的priority Queue:

![](http://coolshell.cn//wp-content/uploads/2012/05/coada1.png)

接下来，就是我们的算法--把这个priority Queue转化成一个二叉树，我们始终从Queue取两个元素来构造一个二叉树(第一个是左节点，第二个是右节点)，把这两个元素的priority相加，并放回到priority Queue中。然后我们得到如下的图表：

![](http://coolshell.cn//wp-content/uploads/2012/05/coada2.png)

同样，我们再把前两个取出来，合并成一个priority为2+2=4的结点，然后放回到priority Queue中:

![](http://coolshell.cn//wp-content/uploads/2012/05/coada31.png)

继续我们的算法，我们会看到，这是一个自底向上建树的过程：

![](http://coolshell.cn//wp-content/uploads/2012/05/coada4.png)

![](http://coolshell.cn//wp-content/uploads/2012/05/coada5.png)

![](http://coolshell.cn//wp-content/uploads/2012/05/coada61.png)

最终，我们会建成这么一棵二叉树：

![](http://coolshell.cn//wp-content/uploads/2012/05/arbore_final.png)

此时，我们把这个树的左支编码为0，右支编码为1，这样我们就可以遍历这棵树得到字符的编码，比如：‘b’的编码是 00，’p’的编码是101， ‘r’的编码是1000。**我们可以看到出现频率越多的会越在上层，编码也越短，出现频率越少的就越在下层，编码也越长。**

![](http://coolshell.cn//wp-content/uploads/2012/05/arbore_final_numerotat.png)

最终，我们会得到下面的这张编码表：

	|字符|编码|
	|b  |00  |
	|e  |11  |
	|p  |101 |
	|"  |011 |
	|o  |010 |
	|r  |1000|
	|!  |1001|
	
当我们来encode的时候，我们是按着bit来encode的，encode也是通过bit来完成。比如我们有这样的bitset: “1011110111″那么，其解码后就是“pepe”。所以，我们需要通过这个二叉树建立我们的Huffman编码和解码的字典表。

这里要注意一点，Huffman算法对各个字符的编码是不会冲突的，也就是说，**不会存在一个编码是另一个编码的前缀**，不然的话问题就大了。

于是，对于我们的原始字符串  beep boop beer!

其对就能的二进制为 : 0110 0010 0110 0101 0110 0101 0111 0000 0010 0000 0110 0010 0110 1111 0110 1111 0111 0000 0010 0000 0110 0010 0110 0101 0110 0101 0111 0010 0010 0001

我们的Huffman的编码为： 0011 1110 1011 0001 0010 1010 1100 1111 1000 1001

从上面的例子中，我们可以看到被压缩的比例还是很可观的。





