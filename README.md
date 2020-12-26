# 歡迎使用CrazyRyan-TOC

## 使用相關程式及工具

### 使用語言
* Python 3.8
### 使用工具(抄自requirements)
```sh
預設程式
certifi==2019.9.11
chardet==3.0.4
click==7.0
flask==1.0.2
future==0.18.1
idna==2.8
itsdangerous==1.1.0
jinja2==2.10.3
line-bot-sdk==1.14.0
markupsafe==1.1.1
pygraphviz==1.5
python-dotenv==0.10.3
requests==2.22.0
six==1.12.0
transitions==0.6.9
urllib3==1.25.6
werkzeug==0.16.0
以下為爬蟲相關
beautifulsoup4==4.8.1
lxml==4.4.2
numpy==1.17.4
pandas==0.25.3
以下為台灣股市相關
twstock==1.3.1
```
## 使用教學
## 遇到的困難及解決方法
因為這次在Ubuntu的環境下進行程式編寫，其實少了很多windows 系統會遇到的問題，這裡還是把這份doc給完備，希望在未來某人作相同專案或類似專案時會有所幫助

#### 我自製的run file
其實這份file只是為了讓我可以在每次git到heroku時不用一直打指令，故特別編寫
```
export PATH=$PATH:/home/ryan/TOC-Project-2020-master
指定專案路徑
heroku git:remote -a crazyryan-toc
指定專案的app名稱
git add .
把全部的檔案都ㄍ
git commit -m "Final commit"

git push -f heroku master

heroku logs --tail -a crazyryan-toc
```

* pygraphviz (For visualizing Finite State Machine)
    * [Setup pygraphviz on Ubuntu](http://www.jianshu.com/p/a3da7ecc5303)
	* [Note: macOS Install error](https://github.com/pygraphviz/pygraphviz/issues/100)


#### 遺失的.env
助教公布的readme裡面有提到這段，要把
You should generate a `.env` file to set Environment Variables refer to our `.env.sample`.
`LINE_CHANNEL_SECRET` and `LINE_CHANNEL_ACCESS_TOKEN` **MUST** be set to proper values.
Otherwise, you might not be able to run your code.
但是，如果直接進行push的話，這個檔案其實會被遺失掉
需要把`.gitignore` 的第85行進行註解後再使用
但要再注意，雖然這份專案的secret被公開並無大礙，但其實公開key在github是危險的，要小心使用


#### Run Locally
You can either setup https server or using `ngrok` as a proxy.

#### a. Ngrok installation
* [ macOS, Windows, Linux](https://ngrok.com/download)

or you can use Homebrew (MAC)
```sh
brew cask install ngrok
```

**`ngrok` would be used in the following instruction**

```sh
ngrok http 8000
```

After that, `ngrok` would generate a https URL.

#### Run the sever

```sh
python3 app.py
```

#### b. Servo

Or You can use [servo](http://serveo.net/) to expose local servers to the internet.


## Finite State Machine
![fsm](./fsm.png)

## Usage
The initial state is set to `user`.

Every time `user` state is triggered to `advance` to another state, it will `go_back` to `user` state after the bot replies corresponding message.

* user
	* Input: "go to state1"
		* Reply: "I'm entering state1"

	* Input: "go to state2"
		* Reply: "I'm entering state2"

## Deploy
Setting to deploy webhooks on Heroku.

### Heroku CLI installation

* [macOS, Windows](https://devcenter.heroku.com/articles/heroku-cli)

or you can use Homebrew (MAC)
```sh
brew tap heroku/brew && brew install heroku
```

or you can use Snap (Ubuntu 16+)
```sh
sudo snap install --classic heroku
```

### Connect to Heroku

1. Register Heroku: https://signup.heroku.com

2. Create Heroku project from website

3. CLI Login

	`heroku login`

### Upload project to Heroku

1. Add local project to Heroku project

	heroku git:remote -a {HEROKU_APP_NAME}

2. Upload project

	```
	git add .
	git commit -m "Add code"
	git push -f heroku master
	```

3. Set Environment - Line Messaging API Secret Keys

	```
	heroku config:set LINE_CHANNEL_SECRET=your_line_channel_secret
	heroku config:set LINE_CHANNEL_ACCESS_TOKEN=your_line_channel_access_token
	```

4. Your Project is now running on Heroku!

	url: `{HEROKU_APP_NAME}.herokuapp.com/callback`

	debug command: `heroku logs --tail --app {HEROKU_APP_NAME}`

5. If fail with `pygraphviz` install errors

	run commands below can solve the problems
	```
	heroku buildpacks:set heroku/python
	heroku buildpacks:add --index 1 heroku-community/apt
	```

	refference: https://hackmd.io/@ccw/B1Xw7E8kN?type=view#Q2-如何在-Heroku-使用-pygraphviz

## Reference
[twstock](https://twstock.readthedocs.io/zh_TW/latest/) - 開源的台灣股市python module，使用了查股價以及簡評的module

[vickyyeh](https://github.com/vickyyeh/Linebot9) - 參考並改寫其中對於匯率爬蟲的code，改寫了輸出型式以及輸入的幣別

[github上傳教學](https://docs.google.com/presentation/d/1y0xIJuEBSTnHCjQ9X1AsHUm_gFtOzrA66-vSgvQAFMc/edit#slide=id.p) - 半年前學長於機械系學會部課所用的教學文件

[助教公佈的ppt](https://docs.google.com/presentation/d/e/2PACX-1vThBHTe2iRVzvead5tBeqnshkhmE61j13rMOs8iwzGgodWheJNlOntg7hXuSlMEY-Ek1l7XA1rzM-xK/pub?start=false&loop=false&delayms=3000#slide=id.p1) - 其中有部份已知錯誤或漏寫的訊息，有在上面遇到的問題進行描述
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM2NjAzNTkxNiwtNzk0NjUyMDU2LDExMz
k2NzY2ODIsNDQ2OTc1NDFdfQ==
-->