# UnityTimeline插件  拓展方法
#### 废话不多说，直接看案例
### 相机震屏案例
+ Behaviour类   ：  行为类，具体用于表示物体在对应的时间做的行为
  + CameraShakeBehaviour.cs
   ```
    using UnityEngine.Playables; // 需要继承的类
    [System.Serializable]
    public class CameraShakeBehaviour : PlayableBehaviour
    {
        ....  //public一些想要开放出来的参数
        ////继承PlayableBehaviour类之后，有对应的生命周期，具体可以进入PlayableBehaviour类去看看        
    }
    ```
    + CameraMixBehaviour.cs   //若是涉及到融合的问题，需要编写一个融合动画的类（用位置移动说明）,同样是继承了PlayableBehaviour类
    ```
      using Unityengine.Playables;//
      public class TransformMixBehaviour{
          //以下这种写法只支持两种动画的融合
          public override void ProcessFrame(Playable playable, FrameData info,object playerData){
              int count = playable.GetInputCount();
              Behaviourl类 dataA ,dataB;
              float dataWeight = 1f;
              for(int i = 0;i<count;i++){
                  float weight = playable.GetInputWeight(i);
                  if(weight < 0.001f) continue;
                  if(dataA == null){
                      dataA = ((ScriptPlayable<对应的behaviour类>)playable.GetInput(i)).GetBehaviour();
                  }else{
                      dataB = ((ScriptPlayable<对应的behaviour类>))playable.GetInput(i)).GetBehaviour();
                      dataWeight = weight;
                      break;///目前只支持两个动画的融合
                  }
              }
              if(dataA == null) return;
              Vector3 pos;
              if(dataB == null){
                  pos = dataA.position
              }else{
                  pos = dataA.position*(1-dataWeight) + dataB.position * dataWeight;

              }
              ///TODO
              ///pos就是两个clip之间的动画融合的后的pos
          }
      }
+ Clip类    ： 动画类，用于在Timeline面板上的动画块显示 
     CameraShakeClip.cs  
    ```
    using UnityEngine.Playable;
    using UnityEngine.Timeline;
    public class CameraShakeClip : PlayableAsset,ITimelineClipAsset{
        //new一个对应的behaviour 对象
        public CameraShakeBehaviour data = new CameraShakeBehaviour();

        //以下函数表示  Clip块的标准长度
        public override double duration{
            get{
                return 1;
            }
        }
        //具体类型直接去ClipCaps类查询
        public ClipCaps clipCaps{
            get{
                return ClipCaps.Blending;
            }
        }
    }
+ Track类  ： 轨道类，用于描述在Timeline面板上的轨道动画,  CameraShakeTrack.cs
  ```
    ///需要引用的类
    using UnityEngine.Playable;
    using UnityEngine.Timeline;
    [TrackClpiType(typeof(对应的clip类))]  // 只有在这里声明之后，才能在Timeline面板上创建对应的clip
    public class CameraShakeTrack: TrackAsset
    {
        protected override Playable CreatePlayable(PlayableGraph graph,GameObject go,TimelineClip clip)
        {
            var p = base.CreatePlayable(graph,go,clip);
            if(clip.displayName == "CameraShakeClip") clip.displayName = "相机震动";//修改clip在timeline面板上的显示
             ///若是要获取对应的clip，可以用以下方法获取
            //var ca = clip.asset as CameraMacroClip;  //获取到对应的clip类
            //if(ca) ca.timelineClip = clip;  
            /// 假设在自定义的clip类里有定义timelineClip这个参数，则可以将clip这个参数赋予到自定义的clip里面
            return p;
        }
        public override void GatherProperties(PlayableDirector director,IPropertyCollerctor driver)
        {
            base.GatherProperties(director,driver);
            if(name == "Camera Shake Track") name = "相机轨道";//修改轨道在Timeline面板上的显示
        }

        ///如果需要混合动画，则需要重写下面的函数
        public override Playable CreateTrackMixer(PlayableGraph graph,GameObject go,int inputCount)
        {
            var p = ScriptPlayable<CameraShakeMixBehaviour>.Create(graph,inputCount);
            return p;
        }
    }
+ 