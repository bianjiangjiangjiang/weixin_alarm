# weixin_alarm
微信群通知
通过https://github.com/eatmoreapple/openwechat 大佬的微信 API接口实现 微信群通知，但存活时间一般就几个小时
接口文档可参考https://openwechat.readthedocs.io/zh/latest/




```golang
package main

import (
	"fmt"
	"openwechat-master"
	"time"
)

'''
func sendMessageToGirlFriend(gf *openwechat.Friend) {
	for {
		now := time.Now()
		t := time.Date(now.Year(), now.Month(), now.Day(), 6, 20, 0, 0, now.Location())
		fmt.Println(t)
		//timer := time.NewTimer(now.Sub(t))
		timer := time.NewTimer(2 * time.Second)
		<-timer.C
		gf.SendText("早安~")
		break
	}
}
'''

func monitor() {
	

}

func main() {
	//bot := openwechat.DefaultBot()
	bot := openwechat.DefaultBot(openwechat.Desktop) // 桌面模式，上面登录不上的可以尝试切换这种模式
	// 注册消息处理函数
	bot.MessageHandler = func(msg *openwechat.Message) {
		if msg.IsText() && msg.Content == "ping" {
			msg.ReplyText("pong")
		}
	}
	// 注册登陆二维码回调
	bot.UUIDCallback = openwechat.PrintlnQrcodeUrl

	// 登陆
	if err := bot.Login(); err != nil {
		fmt.Println(err)
		return
	}

	// 获取登陆的用户
	self, err := bot.GetCurrentUser()
	if err != nil {
		fmt.Println(err)
		return
	}

	// 获取所有的好友
	// friends, err := self.Friends()

	// if err != nil {
	// 	fmt.Println(err)
	// 	return
	// }


	// girlFriend := friends.SearchByRemarkName(5, "边疆2")
	// fmt.Println(girlFriend)
	// if girlFriend.Count() > 0 {
	// 	go sendMessageToGirlFriend(girlFriend.First())
	// }

	//获取所有群组
	groups, err := self.Groups()
	if err != nil {
		fmt.Println(err)
		return
	}


	singegroup := groups.SearchByNickName(1, "message")
	fmt.Println(singegroup)
	if singegroup.Count() > 0 {
		//go sendMessageToGroup(singegroup)
		content := monitor() 

		singegroup.SendText("hello world")
	}

	bot.Block()
}
```
