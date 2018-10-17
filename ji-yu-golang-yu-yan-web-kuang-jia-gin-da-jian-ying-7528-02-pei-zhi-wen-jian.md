# 基于golang语言web框架gin搭建应用-02配置文件\(yml\)

新建`common`文件夹，配置文件采用的`yml`文件，项目中可能用到数据库\(mysql,postgres,redis\)，消息队列等，不同的配置可能名称相同， 故采用了不同的struct来获得不同的配置信息。

## 配置文件

在`common`文件夹下创建`config.go`文件，输入下面内容

    //server config will used in file variables.go
    type configModel struct {
        Server              *serverModel       `yaml:"server"`

    }
    //serverModel get server information from config.yml
    type serverModel struct {
        Mode                 string        `yaml:"mode"`                    // run mode
        Host                 string        `yaml:"host"`                    // server host
        Port                 string        `yaml:"port"`                    // server port
        EnableHttps          bool          `yaml:"enable_https"`            // enable https
        CertFile             string        `yaml:"cert_file"`               // cert file path
        KeyFile              string        `yaml:"key_file"`                // key file path
        JwtPubKeyPath        string        `yaml:"jwt_public_key_path"`     // jwt public key path
        JwtPriKeyPath        string        `yaml:"jwt_private_key_path"`    // jwt private key path
        TokenExpireSecond    time.Duration `yaml:"token_expire_second"`     // token expire second
        SystemStaticFilePath string        `yaml:"system_static_file_path"` // system static file path
    }

在`common`文件夹下创建`variables.go`文件，引入`config.go`文件中用到的struct, `ConfigInfo`为获得的所有配置信息的根，`variables.go`中其他变量\(全局\)都将冲`ConfigInfo`中获得信息，为了便于在别的地方使用，故将他们单独拿出来。

```
var (
    ConfigInfo *configModel // all server config information
)
```

## 配置文件加载

创建`system`文件夹，增加`config.go`文件,`LoadConfigInformation`函数用于获得配置文件的信息,该函数默认会从当前项目的`conf`文件夹加载`config.yml`，获得其中的信息
