# Unity打包相关API
## 一、C#读取xml方式
### 1、从xml中读取int类型的方法
        /// <summary>
        /// 从xml文件中获取对应tag的值
        /// </summary>
        /// <param name="tag"></param>标签名字
        /// <param name="defaultValue"></param>默认值
        /// <returns></returns>
        using System.Xml;
        static int GetXmlNodeInt(string tag, int defaultValue)
        {
            XmlDocument xml_file = new XmlDocument();
            var val = xml_file.GetElementsByTagName(tag);
            if (val == null || val.Count <= 0) return defaultValue;
            int outValue;
            if(int.TryParse(val[0].InnerText,out outValue))
            {
                return outValue;
            }
            return defaultValue;
        }
### 2、从xml中读取string类型的方法
        /// <summary>
        /// 从xml文件中读取对于tag的值（string类型）
        /// </summary>
        /// <param name="tag"></param>
        /// <param name="defaultValue"></param>
        /// <returns></returns>
        static string GetXmlNodeString(string tag,string defaultValue)
        {
            XmlDocument xml_file = new XmlDocument();
            xml_file.LoadXml("具体的地址");
            var val = xml_file.GetElementsByTagName(tag);
            if(val == null || val.Count <= 0)
            {
                return defaultValue;
            }
            return val[0].InnerText;
        }
### 以此类推，获取其他类型的方法也可以写出来了。
## Unity中相关的设置
### 1、Android的数字签名
    /// <summary>
    /// android专用，数字签名
    /// </summary>
    static void SetAndroidSign()
    {
        //设置bundle的版本号
        PlayerSettings.Android.bundleVersionCode = 1;
        //设置keystoreName
        PlayerSettings.Android.keystoreName = "keystore的路径";
        //设置keystore密码
        PlayerSettings.Android.keystorePass = "密码";
        //设置keyaliasName
        PlayerSettings.Android.keyaliasName = "keystore的名称";
        //设置keyalias密码
        PlayerSettings.Android.keyaliasPass = "跟keystore密码一样";
    }
## 2、选择对应的平台
    //自定义平台名称
    public enum PLATFORM
    {
        PC = 0,
        IOS = 1,
        ANDROID = 2,
    }
     /// <summary>
    /// 选择平台
    /// </summary>
    /// <param name="plat"></param>
    static void SelectPlat(PLATFORM plat)
    {
        BuildTarget target = BuildTarget.StandaloneWindows64;
        BuildTargetGroup targetGroup = BuildTargetGroup.Standalone;
        //根据平台选择target和targetGroup
        if(plat == PLATFORM.PC)
        {
            target = BuildTarget.StandaloneWindows64;
            targetGroup = BuildTargetGroup.Standalone;
        }
        else if (plat == PLATFORM.ANDROID)
        {
            target = BuildTarget.Android;
            targetGroup = BuildTargetGroup.Android;
        }
        else if(plat == PLATFORM.IOS)
        {
            target = BuildTarget.iOS;
            targetGroup = BuildTargetGroup.iOS;
        }

        //选择当前平台
        if(EditorUserBuildSettings.activeBuildTarget != target)
        {
            EditorUserBuildSettings.SwitchActiveBuildTarget(targetGroup,target);
        }
    }
### 3、获取打包场景名称
    /// <summary>
    /// 获取打包场景名称
    /// </summary>
    /// <returns></returns>
    static string[] GetBuildScenes()
    {
        List<string> sceneArray = new List<string>();
        foreach(EditorBuildSettingsScene e in EditorBuildSettings.scenes)
        {
            if (e == null) continue;
            if (e.enabled) sceneArray.Add(e.path);
        }
        return sceneArray.ToArray();
    }
### 4、相关的一些设置
#### 1）、 设置com.公司名.包名
    PlayerSettings.SetApplicationIdentifier(BuildTargetGroup,"com.公司名.包名");
#### 2）、设置宏定义
    PlayerSettings.SetScriptingDefineSymbolsForGroup(BuildTargetGroup,"宏定义")；
#### 3）、android专用，设置采用IL2CPP还是Mono
    PlayerSettings.SetScriptingBackend(BuildTargetGroup,ScriptingImplementation.Mono2X /ScriptingImplementation.IL2CPP);
#### 4）、打包
    string[] scenes = GetBuildScenes();//获取用于打包的场景
    BuildPipeline.BuildPlayer(scenes,"输出包路径+输出包名字+后缀",BuildTarget,BuildOptions);