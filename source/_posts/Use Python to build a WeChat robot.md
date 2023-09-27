---
title: Use Python to build a WeChat robot
date: 2023-09-26 19:57:24
categories:
  - Python
tags:
  - Wechat
  - tutorial
description: In this blog, I'm going to teach you how to use python to build a WeChat robot in order to improve you efficieny on using WeChat
cover: https://s2.loli.net/2023/09/27/YxLBecpO7EsazjM.png
---

## Introduction

This program extracts the internal functions of WeChat, then loads Python into the program, and then exports these functions into library functions, which can be used in Python.

When the program starts, it will execute main.py in the py_code directory, similar to when you use `python main.py` on the command line.

Now the py script will be loaded as a plug-in, put the script in the plugins directory, ignore the scripts starting with _, and then load all other py scripts

Plug-in scripts are divided into two categories. py files starting with msg will be loaded in *deal_msg.py*-processing messages, and other scripts will be loaded in *main.py*. If you need to receive a message to do something, name it starting with msg, otherwise just name it casually.

All scripts loaded by *main.py* run in the same thread. If you need multiple threads, please inherit threading.Thread in the script, refer to *check_friend.py*

The program exports a function library that can be used in Python. The library name is wxfunction. For specific functions, see the function introduction below. This library is written for other languages and just exports the interface for Python to use

### Already have a plugin

- Check the status of all friends (blocked, deleted, etc.): check_friend.py
- Monitor group messages and trigger keyword warnings (keyword rules will be added later): msg_monitor_keyword.py
- Send a message every once in a while: _send_msg_timing.py
- Automatically reply to specified friends: msg_auto_reply.py
- Save chat records to postgre database: msg_postgre.py
- Automatic collection
- Automatically receive friend requests
- Automatically save chat files, pictures, etc.
- More plug-ins to be developed

### Built-in functions

- Save all files, pictures, videos, voices and emoticons
- If you want to modify the save path, you can refer to the saved code file.

![](https://s2.loli.net/2023/09/27/OQU2TBbZyrNP7oM.png)

![](https://s2.loli.net/2023/09/27/fS7VDtMmQXixI5K.png)

### Example of sending a message

For example, if you want to send a message every five minutes, the Python code is as follows (after modifying the Python code, you need to close the software and reopen it to take effect. WeChat does not need to log in again)

```
from threading import Timer

def sendmsg(interval):
    '''每隔interval秒给文件传输助手发一次消息, filehelper是文件传输助手的wxid'''
    wxfunction.SendTextMsg("filehelper", "测试消息！")
    timer = Timer(interval, sendmsg, args=(interval,))
    timer.start()

sendmsg(5*60)

```
SendTextMsg is a function exported by the program to send text messages. The first parameter is wxid, which is the only id used internally by WeChat. Each WeChat ID has a corresponding wxid, which can be obtained by getting the friend list. The second parameter is Sent message content

### Receive message example
Processing of received messages, for example, if you want to receive a reply from someone, and then send him a message, it is like a docking robot. The code mainly looks at deal_msg.py, and the startup function is as follows

```
def run(self):
    while self.event.is_set():
        msg = self.wxfunction.popFromMsgQueue()
        if not msg:
            time.sleep(0.5)
            continue
        msg_data = json.loads(msg)
        msg_struct = ChatMsgStruct(**msg_data)
        self._deal_msg(msg_struct)

```
The code is very simple. It pops a message in json format from the program's message queue, then converts it into a class and processes it in the _deal_msg method. The advantage of converting it into a class is that I defined the corresponding fields of the message in the class. When writing code, I can use msg_struct.content to obtain it, and the editor will automatically complete it.

You only need to determine whether the sender's wxid is the person you want to reply to in the _deal_msg method, and then call wxfunction.SendTextMsg to send him a message.

## All features

### Receive messages

- Friend messages
- Group message
- Notification messages (notification of members entering the group, etc.)
- Public account push (can be used to monitor public account posts)
- Official account messages (messages sent by official accounts)
- friend request
- Withdraw reminder message
- Group announcement
- Transfer message
- Payment message 
- Live reminder for the public accounts you follow
- Large file upload completion prompt (when someone else sends the file)
- Discover more news yourself
- Missing message types can also be mentioned

### Send messages

- Send text
- send pictures
- Send File
- Send emoticon
- Send business card
- Send xml message
- Send a photo
- Send applet
- Forward message

### Anti-withdrawal

- Built-in (open the software, it is turned on by default and cannot be turned off)

### Group related
- Get group members
- Get group member nickname
- Delete group members
- Set up group announcements
- Modify group name
- Modify your group nickname
- Invite friends to the group
- add friend
- Agree to friend request (case Python code can be configured to automatically agree)
- Check friend status (refer to the plug-in check_friend.py for usage of this function)
- Search for friends (you can search by mobile phone number or WeChat ID)
- Transfer and receive money
- Receive transfers and return transfers (the case Python code can be configured to receive automatically)
- other
- Modify friend notes
- Get friend details
- Get friends list
- Get relevant information about wxid

### CDN download
- Download pictures
- Download video
- download file
- Download voice
- Download emoticons
- pending upgrade
- Send a quote message
- Send a message
- speech to text
- Get Moments
- Message marked as read

## Step

### Preparation

1. Install a given version (3.9.6.32) of WeChat to any directory
2. Install the given python-3.8.10.exe to any directory. If you don’t understand, the installation options can always be default.
3. Edit the configuration file, mainly modify the WeChat installation directory and Python installation directory
4. Open the wxrobot.exe software, click -"Help-" on the software interface to start WeChat and log in (if you cannot listen to messages, you need to run WeChat with administrator rights before running the software)

Tip 1: The Python version does not need to be the given 3.8.10, newer versions should be available, but they must be 32-bit Python

Tip 2: You don’t have to use software to start WeChat. You can also click the shortcut to start it yourself. However, both the software and WeChat need to be run in administrator mode. If not, the software cannot control other users’ programs.

## All function introduction
### getSelfWxid
Function prototype: `def getSelfWxid() -> str: ...`

Function: Get the wxid of the WeChat you log in to

### getWeChatFilePath
Function prototype: `def getWeChatFilePath() -> str: ...`

Function: Get the saving path of WeChat files (the default saving path of WeChat files in WeChat settings file management)

### GetUsers
Function prototype: `def GetUsers() -> List[dict]: ...`

Function: Get the currently logged in wxid, WeChat ID and nickname

### GetContactList
Function prototype: `def GetContactList() -> List[list]: ...`

Function: Get friend and group list

### popFromMsgQueue
Function prototype: `def popFromMsgQueue() -> Union[str, None]: ...`

Function: Pop a message from the received message queue, the message type is json string

### SendTextMsg
Function prototype: `def SendTextMsg(wxid:str, text:str) -> int: ...`

Function: Send text message

**Parameter:**

1. wxid:  the other party’s wxid
2. text:  text content sent

### SendXmlMsg
Function prototype: `def SendXmlMsg(wxid:str, xml:str, dtype:int) -> int: ...`

Function: Send xml message, the accepted message type is 49, you should be able to take down the xml and resend it. Only tested sending official account articles

**Parameter**:

1. wxid: the other party’s wxid
2. xml: xml content sent
3. dtype: The type in xml can be parsed from xml

### SendEmotionMsg
Function prototype: `def SendEmotionMsg(wxid:str, path:str) -> int: ...`

Function: Send emoticons

**Parameter:**

1. wxid: the other party’s wxid
2. path: the absolute path of the emoticon package, which can be an unencrypted emoticon package or an encrypted emoticon package under FileStorage\CustomEmotion

### SendCardMsg
Function prototype: `def SendCardMsg(wxid:str, xml:str) -> int: ...`

Function: Send a friend’s business card. You can also use the following function to send it directly through the friend’s wxid.

**Parameter:**

1. wxid: the other party’s wxid
2. xml: The xml data of the business card. You can send it first and then print it out in the received message to see it.

### SendCardMsgByWxid
Function prototype: `def SendCardMsg(wxid:str, cardWxid:str) -> int: ...`

Function: Send a friend’s business card

**Parameter:**

1. wxid: the other party’s wxid
2. cardWxid: the friend wxid to be sent

## SendPatMsg

Function prototype: `def SendPatMsg(roomid:str, wxid:str) -> int: ...`

Function: Send a pat message

**Parameter:**

1. roomid: group id
2. wxid: wxid of the person taking the photo

### SendImageMsg
Function prototype: `def SendImageMsg(wxid:str, path:str) -> int: ...`

Function: Send pictures

**Parameter:**

1. wxid: …
2. path:  absolute path of the image sent

### SendFileMsg
Function prototype: `def SendFileMsg(wxid:str, path:str) -> int: ...`

Function: Send files

**Parameter:**

1. wxid: …
2. path:  absolute path of the file sent

### SendAppMsg
Function prototype: `def SendAppMsg(wxid:str, gappid:str) -> int: ...`

Function: Send mini program message

**Parameter:**

1. wxid: …
2. gappid: An ID like gh_xxxxxxxx@app can be forwarded to a small program, and the xml in it is

### ForwardMessage
Function prototype:  `def ForwardMessage(wxid:str, localid:int) -> int: ... `

Function: forward message

**Parameter:**

1. wxid: …
2. localid:  the localid field in the message

### EditRemark
Function prototype: `def EditRemark(wxid:str, remark:str) -> int: ...`

Function: Edit friends’ notes

**Parameter:**

1. wxid: …
2. remark: remark content

### RecvTransfer
Function prototype: `def RecvTransfer(wxid:str, transferid:str, transcationid:str) -> int: ...`

Function: Receive transfer

**Parameter:**

1. wxid: …
2. transferid: can be extracted from the xml of the transfer message
3. transcationid: can be extracted from the xml of the transfer message

### RefundTransfer
Function prototype: `def RefundTransfer(wxid:str, transferid:str, transcationid:str) -> int: ...`

Function: Return transfer

**Parameter:**

1. wxid: …
2. transferid: can be extracted from the xml of the transfer message
3. transcationid: can be extracted from the xml of the transfer message

### GetChatRoomMembers
Function prototype: `def GetChatRoomMembers(roomid:str) -> str: ...`

Function: Get all group members of a group

**Parameter:**

1. roomid: group id

### GetChatRoomMemberNickname
Function prototype: `def GetChatRoomMemberNickname(roomid:str, wxid:str) -> str: ...`

Function: Get group member nicknames

**Parameter:**

1. roomid: group id
2. wxid: The wxid of the nickname to be obtained

### GetUserInfoJsonByCache
Function prototype: `def GetUserInfoJsonByCache(wxid:str) -> str: ...`

Function: Get the nickname of a user, which can be a friend or group member

**Parameter:**

1. wxid:  The wxid of the nickname to be obtained

### DelChatRoomMembers
Function prototype: `def DelChatRoomMembers(roomid:str, wxid:str) -> int: ...`

Function: Delete group members

**Parameter:**

1. roomid:  group id
2. wxid:  The wxid of the person to be deleted

### SetChatRoomAnnouncement
Function prototype: `def SetChatRoomAnnouncement(roomid:str, content:str) -> int: ...`

Function: Set group announcement

**Parameter:**

1. roomid:  group id
2. content:  Announcement content, only text content is supported

### SetChatRoomName
Function prototype: `def SetChatRoomName(roomid:str, name:str) -> int: ...`

Function: Modify group name

**Parameter:**

1. roomid:  group id
2. name

### SetChatRoomMyNickname
Function prototype: `def SetChatRoomMyNickname(roomid:str, name:str) -> int: ...`

Function: Modify your own group nickname

**Parameter:**

1. roomid: group id
2. name

### AddChatRoomMembers
Function prototype: `def AddChatRoomMembers(roomid:str, wxid:str) -> int: ...`

Function: Invite friends process, this interface only supports groups with less than 40 people

**Parameter:**

1. roomid: group id
2. wxid: friend’s wxid

### DownloadImageFromCdnByLocalid
Function prototype: `def DownloadImageFromCdnByLocalid(localid:int, file_path:str) -> int: ...`

Function: Download the picture of a certain picture message to the specified path

**Parameter:**

1. localid: localid of the message
2. file_path: The path is generally the WeChat directory, and then decrypted and copied. See the Python example for details

### DownloadFileFromCdnByLocalid
Function prototype: `def DownloadFileFromCdnByLocalid(localid:int, file_path:str) -> int: ...`

Function: Download a file message to the specified path

**Parameter:**

1. localid: localid of the message

2. file_path: The path is usually the WeChat directory, and then copied. See the Python example for details

### DownloadVideoFromCdnByLocalid
Function prototype: `def DownloadVideoFromCdnByLocalid(localid:int, file_path:str) -> int: ...`

Function: Download a video message to the specified path

**Parameter:**

1. localid: localid of the message
2. file_path: The path is usually the WeChat directory, and then copied. See the Python example for details

### AddFriendByWxidOrV3
Function prototype: `def AddFriendByV3V4(v3:str, v4:str, addType:int) -> int: ...`

Function: Agree and request. v3, v4 and addType are all in the message xml of the friend request. See the Python example for specific fields.

**Parameter:**

1. v3: …
2. v4: …
3. addType: The type to add, for example, if added through wxid, it is 6, if added through business card, it is 17.

### getVoiceByMsgid
Function prototype: `def getVoiceByMsgid(msgid:str) -> str: ...`

Function: Obtain voice files through msgid. The format obtained is slik, which needs to be converted to common audio formats such as mp3 to play.

**Parameter:**
1. msgid: msgid in the audio message

### CheckFriendStatus
Function prototype: `def CheckFriendStatus(wxid:str) -> dict: ...`
Function: Detect friend status, block, delete, etc. It can be used to detect zombie fans. It is recommended to call to increase the interval. A prompt to add a friend may appear, which is normal and cannot be seen by the other party.

**Parameter:**

1. wxid: …

### SearchFriend
Function prototype:  `def SearchFriend(phone:str) -> dict: ... `

Function: Search users through WeChat ID or mobile phone number

**Parameter:**

1. phone:  WeChat ID or mobile phone number to be searched

### Get my wxid
Function prototype:` def GetMyWxid() -> dict: ...`

Function: Get the developer’s wxid

Return value: kanadeblisst