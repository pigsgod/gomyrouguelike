# gomyrouguelike
manager


项目演示：


https://github.com/pigsgod/gomyrouguelike/assets/44769072/c8b24e10-9622-4f46-a990-8eb63bd1e654

项目说明
1
目标：通过拓展编辑器实现创建地图
实现效果： ![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/12c21732-5632-4bc2-92de-9d70db474d58)

地图主要数据类及其关系：
![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/13eaf5f0-c2ab-4505-aef5-ae78a8f46c4b)

目录结构：
 ![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/e6b7be1c-edf8-4fef-bceb-268bde5aa4ef)

房间地图数据配置目录：
 ![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/ba487fc6-f9e1-4cb5-907b-9aa973909d38)


2 
目标：通过瓦片地图制作房间
实现效果及层级关系： ![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/5a146d21-c702-4b3e-a6bd-bb53c64b9e8c)


每个瓦片房间分成好几层，小地图，碰撞层，装饰，地板等。
然后存成预制体方便创建各种房间
碰撞层使用复合碰撞体：复合碰撞体可以模拟游戏对象的形状，同时保持较低的处理器开销。
瓦片房间目录： ![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/dc1d496a-ea75-490c-aebf-d64af142bb0d)

![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/ec2e90ae-ac30-4cba-89e5-55cc7d2460b1)
 创建瓦片房间数据类（存储瓦片房间预制体及其相关信息比如怪物生成点与房间门口位置等）
瓦片房间数据对象目录 
![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/6d7bac6f-7653-4b60-8077-2bf953c3342a)

创建各个方向门口瓦片门口，步骤与瓦片房间基本一致 
3
目标：根据1与2的地图与瓦片房间生成伪随机的实际地图
实现效果： ![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/e2ba038f-834d-443a-add5-83743745667a)

先创建 地牢关卡信息so类及对象，用于存储该关卡所有的地图数据对象，与该关卡 所有的瓦片房间数据对象。
![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/33625323-1908-4837-bd9c-79c5dcc79230)

 ，即一个关卡可以有多个地图，一个关卡也对应多个瓦片房间
![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/ffb4864c-ad42-436c-a4f6-f87072a26dd1)

![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/df7d65c4-a291-4089-9373-6dbfa83ffe00)

 
 
3
目标：制作玩家及其动画（敌人制作同理）
![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/de109c54-f75b-40e9-a7d4-89e77efda5f0)

玩家预制体层级关系 
Polygon collider2d组件 和 BoxCollider2d组件：为什么要两个：为了针对不同的碰撞情况作出反应，一个物体可以通过放置多个碰撞体实现
Sorting group组件作用：一个对象如果包含很多个子对象比如手，脚等，那么需要在其Sprite Renderer组件中通过同一排序图层 (Sorting Layer)，但是具有不同的 Order in Layer 值进行排序，但是此时会出现两个预制件的精灵各部位相交问题

实现思路： ![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/816bc4a8-815d-4876-b6e5-bb4af3645de4)

Tips：通过CinemachineTargetGroup组件中实现摄像头跟随角色与鼠标
4
目标：通过发布订阅模式对玩家各种事件的控制
 通过xxxevent创建事件，然后xxx去订阅，最后通过playercontrol去发布调用事件达到解耦
![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/58a7d63c-ed30-4877-bbc1-7c03481a6865)

5
目标：实现对象池
 ![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/ea4a9310-5fc8-4418-9be7-089ecb7bb08f)

6
目标：制作子弹及其动画
实现基本与人物差不多
 
![image](https://github.com/pigsgod/gomyrouguelike/assets/44769072/a851cb40-75e7-4934-8add-f2e5111a2e3e)

7
目标：右上角实现小地图
Step1添加摄像机游戏对象，设置好大小，将渲染图层设置为minimap图层，添加2d灯光，渲染设置为minimap以及camera2，设置摄像机输出到Minimap Render Texture

Step2 添加人物标志游戏对象（设置为minimap层），在其父对象添加脚本（设置人物标志游戏对象头像位置跟着与人物位置一致，并设置改人物为摄像机跟随对象）

Step3 添加ui图片放在右上角，然后图片设置为Minimap Render Texture
8
目标：
实现A星算法

Step1 编写单个瓦片路径信息类，和单个地图所有瓦片信息类
Step2在setting中添加不同瓦片路径权重变量值，在Gameresources中添加可以走的网格和不可以走的网格图片变量值，并在房间初始化时添加 计算所有网格的权重值的数组的方法，用于编写a星算法类

迪杰斯特拉算法（在广度优先搜索思想基础上，进入下一轮遍历时，优先选择与起初节点路径最小的节点进行下一轮） 是针对 图 数据结构的 对于最短路径问题 的 一种算法，A星算法 是 对 迪杰斯特拉算法 优化，引入了对目标节点的最优路径的选择，比起迪杰斯特拉算法只有对起始节点路径最优选择更胜一筹



