# unity

### 1. 刚体（RigidBody）(属性基本与)
* 属性
  > mass（质量）, Drag（平移阻力）, Angular Drag（旋转阻力），Use Gravity（使用重力），Is Kinematic（遵循运动学），Interpolate（插值），Freeze Rotation（冻结旋转），Collision Detection（碰撞检测模式）
* __Is Kinematic（遵循运动学）__  该属性表示对象是否遵循牛顿运动学物理定理。如果该属性设置为true表示该物体运动状态不受外力，碰撞和关节的影响，而 __只受到动画以及附加在物体上的脚本影响__ ，但是该物体仍然能改变其他物体运动状态，例如游戏中倒下的敌人始终不动 ，就是利用这个属性 。
* __Interpolate(插值)__  还属性表示的是该物体运动的插值模式，默认状态下是被禁用的。选择该模式时，在此模式下物理引擎会在物体的运动帧之间进行插值，使得运动更加自然。另外插值导致了物理模拟和渲染的不同步，进而产生物体轻微抖动现象，建议可以对主要角色使用插值，而其他的则禁用此功能，以达到折中的效果。 _(None表示没有插值，Interpolate表示根据上一桢的位置来做平滑插值，Extrapolate表示根据预测的下一桢的位置来做平滑插值)_
* __Collision Detection(碰撞检测模式)__ 默认状态时Discrete。在没有发生碰撞检测的情况下，碰撞物体会穿过对方，产生所谓 穿透现象。碰撞模式有不连续模式（Discrete），连续模式（Continuous）和动态连续模式（ContinuousDynamic），动态连续模式适用于高速运动的物体，连续模式仅仅可以用于球体，胶囊和盒子碰撞者的刚体，而且会严重影响物体的运动表现，因此大部分采用不连续模式。 
  > 离散模式：穿过并不触发碰撞
  > 连续模式：穿过但触发碰撞
  > 动态连续模式：不能穿过并触发碰撞
  
### 2. 射线（Ray，RaycastHit）
* ray类为射线类，RaycastHit用于储存射线碰撞的信息
* 创建一条射线Ray需要指明射线的起点（origin）和射线的方向（direction）。这两个参数也是Ray的成员变量。注意，射线的方向在设置时如果未单位化，Unity 3D会自动进行单位归一化处理。
```
public static bool Raycast(Vector3 origin, Vector3 direction, float distance=Mathf.Infinity, intlayerMask=DefaultRaycastLayers); 
public static bool Raycast(Vector3 origin, Vector3 direction, RaycastHit hitInfo, float distance =Mathf.Infinity, int layerMask = DefaultRaycastLayers); 
public Ray ScreenPointToRay(Vector3 position);
```
* hit中有name，normal等，还有碰撞三角形片元的纹理（两个），光照纹理，collider，rigidbody等

### 3. 碰撞
