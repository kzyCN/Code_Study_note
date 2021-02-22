# Springboot实现邮件发送

## 1.项目的创建

### 1.1创建一个Springboot项目

核心pom.xml的配置

```xml
<!--邮件服务-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
```

### 1.2配置邮箱服务配置文件

在application.yml里面配置如下

下面以QQ邮箱为例：

```yml
spring:
#    邮件服务
  mail:
    host: smtp.qq.com
    username: <qq邮箱账户>
    password: <smtp服务的授权码>
    # 端口号465或587
    port: 587
    # 默认的邮件编码为UTF-8
    default-encoding: UTF-8
    # 配置SSL 加密工厂
    properties:
      mail:
        smtp:
          socketFactoryClass: javax.net.ssl.SSLSocketFactory
        #表示开启 DEBUG 模式，这样，邮件发送过程的日志会在控制台打印出来，方便排查错误
        debug: true
```

![](https://gitee.com/kzycn/picCloud/raw/master/2021/20210130120801.png)

## 2.核心业务代码

### 2.1在service层创建MailService.java

```java
@Service
public class MailService {

    @Autowired
    private JavaMailSender mailSender;

    //传入邮箱发件人
    @Value("${spring.mail.username}")
    private String sender;


   //发送简单邮件
    public void sendSimpleMail(String receiver,String subject,String context){

        SimpleMailMessage simpleMailMessage = new SimpleMailMessage();
        simpleMailMessage.setFrom(sender);
        simpleMailMessage.setTo(receiver);
        simpleMailMessage.setText(context);
        simpleMailMessage.setSubject(subject);
        simpleMailMessage.setSentDate(new Date());

        try {
            mailSender.send(simpleMailMessage);
            System.out.println("简单邮件已发送");
        }catch (Exception e){
            System.out.println("简单邮件发送出错"+e);
        }

    }

    //发送HTML邮件
    public void sendHtmlMail(String receiver,String subject,String context){
        MimeMessage mimeMessage = mailSender.createMimeMessage();
        try {
            MimeMessageHelper mimeMessageHelper = new MimeMessageHelper(mimeMessage, true);
            mimeMessageHelper.setFrom(sender);
            mimeMessageHelper.setTo(receiver);
            mimeMessageHelper.setText(context,true);
            mimeMessageHelper.setSubject(subject);
            mailSender.send(mimeMessage);
        } catch (MessagingException e) {
            System.out.println("简单邮件发送出错"+e);
        }
    }

}

```

### 2.2在controller创建mailController.java

```java
@Controller
public class mailController {

    @Resource
    MailService mailService;

    @Resource
    TemplateEngine templateEngine;

    @RequestMapping("/tomail")
    @ResponseBody
    public String toMail(){
        String title = "<邮件标题>";
        String email = "<收件人邮箱>";
        Context context = new Context();
//        设置传入模板的页面的参数 参数名为:id 参数随便写一个就行
        context.setVariable("id", "000");
//        emailTemplate是要发送的模板邮件
        String process = templateEngine.process("emailTemplate", context);
        mailService.sendHtmlMail(email,title,process);
        return "success";
    }

}

```

