# ✨项目介绍
完美校园电费不足时宿舍用电剩余额度阈值提醒完美校园剩余电费查询
如果你也困扰于宿舍突然停电无提醒的情况可以使用本方法通过设置阈值,当宿舍剩余用电额度不足时,可以发送短信,微信,QQ,钉钉等提醒具体方法,重复获取cookie查询用电额度 比较额度是否低于阈值然后发出提醒,完美校园查询电费+Pushplus推送
# 💃项目背景
学校电费突然停电,当你的宿舍有台式机的时候,心里面还是有点虚的,虽然宿舍楼底有面板提醒某某寝室电费不足,但是提现的不够具体,所以直接写了一个服务,查询自己寝室的剩余用电量当低于5度时自动发送消息到短信wx或qq钉钉里面,这样麻麻再也不用担心你的宿舍停电了
# 🔰项目功能
 完美校园模拟登录获取 token
 可单人自定义推送，也可统一推送
 支持 qq 邮箱、Qmsg、Server 酱、PipeHub 推送打卡消息
# 🎨配置文件
本脚本使用虚拟 id 来登录，如果使用了本脚本就不要再用手机登录 app 了，如果一定要用 app 请不要使用当前脚本。
由于完美校园就设备做了验证，只允许一个设备登录，获取本机的 device_id 可参考https://lingsiki.lanzoui.com/iQamDmt165i（获取方法：蓝奏云，下载解压使用）
```
import requests
import time
 
#推送服务
def Pushplus():
    token = '89a735e1282f4028afe8734fadb8ac39'  # 在pushplus网站中可以找到
    title = '今日电量'  # 改成你要的标题内容
    url = 'http://www.pushplus.plus/send?token=' + token + '&title=' + title + '&content=' + content
    requests.get(url)
 
#访问电量查询网站
url = 'http://h5cloud.17wanxiao.com:8080/CloudPayment/user/getRoomState.do?payProId=2523&schoolcode=843&businesstype=2&roomverify=1-6--56-6415'  #获取信息的Url，6415是房间号
headers = {
    'User-Agent': 'Mozilla/5.0 (Linux; Android 7.1.2; M6 Note Build/N2G47H; wv) AppleWebKit/537.36 (KHTML, like Gecko) '
                  'Version/4.0 Chrome/86.0.4240.99 XWEB/4317 MMWEBSDK/20220903 Mobile Safari/537.36 MMWEBID/300 '
                  'MicroMessenger/8.0.28.2240(0x28001C35) WeChat/arm64 Weixin Android Tablet NetType/WIFI '
                  'Language/zh_CN ABI/arm64',
    'Referer': 'http://h5cloud.17wanxiao.com:8080/CloudPayment/bill/selectPayProject.do?txcode=2&interurl'
               '=substituted_pay&payProId=2523&amtflag=0&payamt=100&payproname=%E7%94%A8%E7%94%B5%E6%94%AF%E5%87%BA'
               '&img=http://cloud.17wanxiao.com:8080/CapecYunPay/images/project/img-nav_2.png&subPayProId= ',
    'cookie': '#你的完美校园cookie'
}
req2 = (requests.get(url, headers=headers)).text    #用于判断是否要更新cookie
req = (requests.get(url, headers=headers))
 
yesterday: float = 0.0
today: float = 0.0
while True:
 
    if 'code=ERROR' in req2:
        print("请更新Cookie")
    else:
        #提取电费信息
        yesterday = today
        power = float(req.json()['quantity'])   #电量
        today = power
        room_number = (req.json()['description'])   #房间号
        power_sum = yesterday - today   #使用电量
        content = ("你的房间号：{0}\n\n昨天查询电量：{2}，今天查询电量：{3}，一日内使用的电量：{4}\n\n剩余电量：{1}度".format(room_number, power,yesterday,today,power_sum))
        #print(notice)
        #推送服务——Pishplus
        Pushplus()
        time.sleep(86400)   #延迟86400秒
```
