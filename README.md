# yii2-captcha


yii2框架的Js验证码，所有的使用方式跟系统自带的yii\captcha\Captcha一致。

本代码主要使用极验的服务，并进行yii2封装，使得所有的代码开放极为简单！感谢极验团队提供的免费服务，同时也欢迎购买其付费服务！


安装
------------

推荐的方式是通过composer 进行下载安装[composer](http://getcomposer.org/download/)。

```
composer require lengnuan-v/yii2-geetest
```

到你的`composer.json`文件中的require段。


使用
--------------
1. 在配置文件中（如common/config/main.php）加入如下的代码：
```php
return [
    'components' => [
        'geetest' => [
            'class' => 'lengnuan\captcha\Geetest',
            'gtId' => '极验的ID'
            'gtKey' => '极验的Key',
            ],
        ],
    ]
];
```
2. 在LoginForm中加入captcha的rules, LoginForm的示例可能如下：
```php
class LoginForm extends Model
{
    public $phone;
    public $smsCaptcha;
    public $captcha;
    public $rememberMe = true;

    // ...其它代码

    public rules()
    {
        return [
            // 该规则类似于\yii\captcha\CaptchaValidator,会将post过来的数据自动去极验后台校验
            ['captcha', '\lengnuan\captcha\GtCaptchaValidator'],
            // ...其它规则
        ],
    }

    public function login()
    {
        //具体登录代码...
    }

    public function sendSms()
    {
        //具体发送验证码代码...
    }
}
```
3.在前端页面(如login.php)上加上captcha的展示:
```php

    use lengnuan\captcha\GtCaptcha;
    <?=  GtCaptcha::widget([
        'onSuccess' => 'function(result){
            sendSms(); // js验证码成功时的动作，本例中为js发出短信验证码
        }',
        'type' => GtCaptcha::TYPE_BIND, //type为绑定，意思为当点击id为bindTo配置(即sendsms)时，调出极验验证框
        'bindTo' => 'sendsms',
        'name' => 'LoginForm[captcha]', // input的name
        'gtOptions' => [                //gt的显示option
             'width' => '50%',
        ],
    ]);?>
```
如上即为示例中的widget的展示，更多配置请参照本文中下面的配置章节

4.书写相应的controller,该内容跟本包无关，但是需要调用相应LoginForm的validate()方法。

经过如上的配置后，你会发现，当你点击按钮时，会直接掉起js校验，后端和前端的校验均已经OK！
