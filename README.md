# Onlinelearning-certification-system-blockchain
Realizing a simple online learning authentication management system by JAVA

上传文件中包含java project：Onlinelearning-certification-system-blockchain以及gson-2.6.2.jar文件
1. 下载好这两个文件后，解压Onlinelearning-certification-system-blockchain.zip，进入Eclipse，点击import existing projects选中下载好的java project打开程序(如若进入程序显示空白页，则需双击每一个.java程序页面重新开启)
2. 在Eclipse menu里打开Windows >preferences, 选择Java >Build path > User Libraries. 点击 new 建立一个新的User Library 取名为 “gson_lib” 点击确认. 选中gson_lib，按右边添加外部的JARs找到你存的gson-2.6.2.jar 应用并关闭
3. 添加User Libarary到你的java project中：右键你的java project的package选择 package explorer > Build path > Add Libraries. 选择User Libraries中你新建的 “gson_lib” 然后点击结束在代码中使用它需Import，Import it with import com.google.gson.*;
4.进入程序页面，选择到类Blockchain中，点击运行即可。

Java代码具体实现

类简述
1.	Block
Block类定义为区块链中的区块，该类拥有以下属性：hash(本区块的哈希值)，previousHash（上一区块的哈希值），index（本区块在链中的序号），merkletreeroot（本区块默克尔树根的哈希值），timeStamp（本区块的时间戳），nonce（用于POW的随机值）。Block类的构造函数有三个输入参数，分别为merkletreeroot（本区块默克尔树树根哈希值），previoushash（上一区块的哈希值），index（本区块在链中的序号），在构造函数中分别将这三个参数赋值给this指针的this.merkletreeroot, this.previousHash, this.index，同时调用Date().getTime()函数生成时间戳赋值给this.timeStamp，调用本区块中的成员函数calculateHash()函数计算本区块的哈希值，将计算好的哈希值赋值给this.hash。
2.	getShavalue
getShavlue类用于定义用于计算哈希值的函数，我们将其命名为applySha256，在之后的函数部分进行进一步介绍。
3.	MerkleTrees
MerkleTrees类为区块中用于存储数据的数据结构，该类拥有以下属性：CourseList（需要存储课程简介构成的列表），root（以哈希值形式呈现的默克尔树根），index（默克尔树在链中的序号）。Merkletrees类的构造函数有两个输入参数，分别为CourseList（需要输入的课程简介列表）和index（本树在链中的序号），在构造函数中分别将这两个参数各自赋值给this指针的this.CourseList和this.index，同时将root属性赋值为空。
4.	BlockChain
BlockChain类用于建造一条区块链，在该类中，区块链以区块（Block）的动态列表形式呈现，同时我们还为默克尔树（MerkleTrees）建立了动态列表，方便之后进行数据的抓取。我们将主程序编写在这个类中，实现了区块的创建，添加数据，成链等区块链建立操作。最后我们设置了成员函数isChainValid()来检验该区块链是否有效。

主要函数简述
1.	applySha256()
applySha56()函数为getShavalue类的公有成员函数，输入为给定string类字符串，输出为计算好的string类哈希值。该函数通过调用java中MessageDigest对象，选择“SHA-256”加密方法，对输入类字符串进行加密，得到唯一与输入字符串所对应的的哈希值字符串。
2.	calculateHash()
calculateHash()为Block类的公有成员函数，用于计算该区块的哈希值，该函数调用getShavalue类的公有成员函数applySha256，将applySha256的输入参数设定为该区块的所有属性值：hash(本区块的哈希值)，previousHash（上一区块的哈希值），index（本区块在链中的序号），merkletreeroot（本区块默克尔树根的哈希值），timeStamp（本区块的时间戳），nonce（用于POW的随机值）。最后得到一个计算好的字符串类哈希值作为函数的返回值。
3.	mineBlock()
mineBlock()函数为Block类的公有成员函数，用于对区块进行挖矿POW证明。该函数的输入参数为int类型自定义的difficulty（挖矿难度），函数通过改变区块属性中的nonce（随机值），循环计算哈希值，直到该哈希值前端拥有匹配自定义的difficulty数值的0。例如将difficulty设置为2，则要求输出的哈希值前端至少有两个零，反之改变nonce值，直到达到这一要求，则挖矿成功。
4.	getNewCourseList()
getNewCourseList()函数为MerkleTrees类的私有成员函数，以字符串类型的列表为输入参数，输出进行过一次合并的字符串类型列表。该函数的功能为依次将默克尔树中在同一
默克尔树示意图
层的节点，按照顺序两两进行合并后计算哈希值形成上一层的父节点。如图所示为例，该函数将N0,N1合并计算哈希值输出给上层列表中的第一个值N4,再将N2,N3合并计算哈希值输出给上层列表中的第二个值N5,至此该层列表计算完毕。我们注意到，如果遇到奇数个节点，则补空值与最后一个子节点进行哈希计算从而生成父节点。
5.	merkle_tree()
merkle_tree()为MerkleTrees类的公有成员函数，用于对存储了数据的动态列表进行迭代计算，得到默克尔树树根的哈希值同时赋值给该默克尔树的root属性值。函数首先将该默克尔树的CourseList中的值依次进行哈希值计算，得到hashList，然后利用上面定义的getNewCourseList()函数进行迭代计算知道目标列表中只剩下一个值，也就是该默克尔树树根的哈希值，最后将该哈希值赋值给默克尔树的root属性值。
6.	isChainValid()
isChainValid()是BlockChain类的公有成员函数，该函数的功能是用来检验已生成的区块链的有效性。首先检验每一个区块的哈希值是否计算正确，然后检验每个区块的前一个区块的哈希值是否等于本区块中所存储的前一个区块的哈希值，同时还需要检验每一个区块的哈希值是否符合给定difficulty的要求从而完成工作量证明，最后通过比对每一个区块中的merkletreeroot与默克尔树列表中每一颗树的树根哈希值从而检验区块链中的数据是否被篡改。

主程序功能简述
主程序定义在BlockChain类中，用于创建默克尔树和区块，同时将区块添加成链等操作。首先我们创建两个动态列表命名为blockchain和merkleTrees分别用来存储区块和默克尔树。然后对于一个新区快，我们首先创建动态CourseList用来存储需要存储的课程简介，然后将该动态列表和序号赋值给一颗新的默克尔树类，利用默克尔树类中的公有成员函数merkle_tree()对该树进行迭代计算得到树根哈希值。在得到了树根哈希值之后，我们将它和previoushash，区块序号作为区块类（Block）的构造函数的参数赋值给构造函数从而创建新区块，同时添加到blockchain动态列表，最后进行挖矿，挖矿成功后，区块完成上链。在添加完所有区块以后，我们利用BlockChain类中定义的公有成员函数isChainValid()来检验该区块链是否有效，通过检验后，我们对该区块链和默克尔树种所存储的数据进行输出，总体流程如下。
