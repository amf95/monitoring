

**- Create a bot on telegram:**
**- Get the bot token:**
**- Create a group or a channel on telegram:**
**- Get the group or channel ID:**
```yaml
# to get chat_id there are two ways:
  # 1- for a group:
  #   1.1- get the -4657986987 part from the group web url 
  #   exp: https://web.telegram.org/k/#-4657986987
  #   2.2- result should look like this -> chat_id: -4657986236
  # 2- for a channel:
  #   2.1- get the -2423304347 part from the channel web url
  #   exp: https://web.telegram.org/k/#-2423304347
  #   2.2- add 100 after the - sign.
  #   2.3 result should look like this: chat_id: -1002423304347
```

**- Add telegram configs to alertmanager.yml file:**

```yaml
receivers:
  - name: 'multi-receivers'
  - 
    telegram_configs:
	  # add you bot token here.
    - bot_token: 7349351759:AAGdGxqNDQQ_RHYEvBDF1PuFZYzx0KFqsPc # dummy data
      api_url: https://api.telegram.org
    
      # group id.
      chat_id: -4657782236 # dummy data
	   # or
       # channel id.
#      chat_id: -1002436704735 # dummy data
```

