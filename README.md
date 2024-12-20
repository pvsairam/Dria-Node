![image](https://github.com/user-attachments/assets/28d801d4-b863-4f55-af14-cdbcc65c10b4)

# Dria Node
* Official Guide + Windows Version: https://dria.co/join
* This guide is for Linux AMD (aka VPS)

## 1- Install Dependecies
```console
# Docker
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo chmod +x /usr/local/bin/docker-compose

# Docker version check
docker --version
```
```console
# Ollama
curl -fsSL https://ollama.com/install.sh | sh
```

## 2- Install Dria
```
cd $HOME
curl -L -o dkn-compute-node.zip https://github.com/firstbatchxyz/dkn-compute-launcher/releases/latest/download/dkn-compute-launcher-linux-amd64.zip
```
```
unzip dkn-compute-node.zip
```
```
cd dkn-compute-node
```

## 3- Run Dria Node
```
./dkn-compute-launcher
```
**1. Enter your DKN wallet Secret key (Your metamask Private Key without `0x`)**

**2. Pick a Model**
* I've picked three models (`Gemini` and `Openrouter`) by writing `10,25`
* `Gemini` is a google API with upto 1500 free requests daily, No cost, doesn't need VPS resources. Get your Google API [here](https://aistudio.google.com/app/apikey)
* `OpenRouter` is an API, You can buy credits with crypto to use it, Get API [here](https://openrouter.ai/settings/keys), I think this one gives better points
* `Ollama` downloads and runs a local model host locally in your server, No cost, but it uses your VPS or system resources. It needs a powerfull VPS, you can ignore and only use Openrouter and Gemini
* `OpenAI` is an API but it needs a monthly payment which means it may not give you points if you use free version, if you have chosen it, get your API [here](https://platform.openai.com/api-keys)

![image](https://github.com/user-attachments/assets/6f8bc7d5-f189-4562-819a-082a66ac476b)


**3. Skip Jina & Serper API key by pressing Enter**

**4. Now your node will start Downloading Models files and Testing them**
**each model must pass its test and it only depends on your system specification**

> Error: If you had any **port** conflicts, you must change the ports in `.env` file or use this: `nano $HOME/dkn-compute-node/.env`

When the Node started logging, Head back to the first lines of the logs and check if both `Ollama` and `OpenAI` passed their tests and see their name in front of `using models` like mine in the following picture:

![Screenshot_389](https://github.com/user-attachments/assets/7d016234-817e-4eef-9542-e256b4f68b7c)

*If you didn't pass any of the Models:*
> - I've chosen lightest `Ollama` model but if it didn't pass the test for you, you can ignore and continue using `OpenAI`
> 
> - Since I wanted to use `Ollama` too because it's free, so I had to stop some of my other running dockers, then restart the Dria script to pass the test (Testing `Ollama` highly depends on your system resources, after testing is passed, it keeps running with very low resources)
> 
> - If you wanted to restart the node to *change* Models, you can clear `DKN_MODELS=` variable in `.env` file or use `nano $HOME/dkn-compute-node/.env`, then rerun Dria using `./dkn-compute-launcher`

## 4- Re-run Dria Node
Now you ensured that your Models passed the test and your node is running, you should re-run your node to start it in a screen. press `Ctrl+C` and exit the node

```
screen -S dria
```
```
./dkn-compute-launcher
```

![image](https://github.com/user-attachments/assets/7ca9f116-50e5-4649-b924-ba4c37b7832c)

![image](https://github.com/user-attachments/assets/5582a204-c232-4f31-9c9f-d215cd0004f3)

You can minimze the screen with `CTRL+A+D`

To open your screen again:`screen -r dria`


## 5- Earn Node-Keeper Role
Join [Discord](https://discord.gg/dria) and Fill the [Form](https://docs.google.com/forms/d/e/1FAIpQLSeK090ejc4dg5x1ztb_yAOxGz5o1V8JUqDa-o3AwV1Lq7NpMA/viewform
) to receive role

![image](https://github.com/user-attachments/assets/4850511f-e9f7-4d5a-9270-2a2613439cc1)

## 6- Check your points
https://steps.leaderboard.dria.co/

#

## Optional: Update Node
### 1- Delete Old files
```
cd $HOME
rm -rf dkn-compute-node.zip
rm -rf dkn-compute-node
```

### 2- Stop Nodes
**Stop Ollama Node**
```console
pgrep ollama
# Example: if 74877, then use:
kill 74877

# OR

sudo systemctl stop ollama
sudo systemctl disable ollama
```

**Stop Dria (Terminate screen)**
```console
screen -XS dria quit
```

### 3- Update and Rerun node
Start from Step [Install Dria](https://github.com/0xmoei/Dria-Node/edit/main/README.md#install-dria)
