# xtra-backend
Backend API for creating Xtra traces

![image](https://user-images.githubusercontent.com/43552143/134864031-8255b329-e9c0-4f20-a8df-92c05e14d2e0.png)



```
sudo apt install zlib1g-dev
```

------

GET http://localhost:8081/trace

ReqBody: raw / JSON

{
    "prog": "let fact = \\x -> case x of 0 -> 1; y -> y * fact (x - 1); in fact 5",
    "filter": "hide <let fact = _X in _Y => _Z>\nhide <fact _X => _Y> then children\nfactor binding\nfactor <_X - 1 => _Y> then all\nhide pattern\nhide (nonFirstTwo <fact _X => _Y>) except (afterLast <fact _X => _Y>)\nhide reflexive"

}

------

From scratch:

sudo apt install -y gcc build-essential curl libffi-dev libffi7 libgmp-dev libgmp10 libncurses-dev libncurses5 libtinfo5 hpack zlib1g-dev
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
    
set up ssh

curl -LO http://archive.ubuntu.com/ubuntu/pool/main/libf/libffi/libffi6_3.2.1-8_amd64.deb
sudo dpkg -i libffi6_3.2.1-8_amd64.deb

curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh
    
git clone git@github.com:Barnesjt/xtra-backend.git
cd xtra-backend/
git submodule init
git submodule update
cd Xtra
hpack
cd ..
cabal run server
