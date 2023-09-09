# GoLang
### Advantages of golang
1. Faster like C and C++ as code is compiled to machine code
2. Concurrency - Built-in support for goroutines and channels
3. Cross-Compilation - Can create binaries for both Windows/Linux
4. Garbage collection - Automatic memory management
5. Modularity - Built-in support for managing dependencies

###### Install:
```
sudo apt update
sudo apt -y upgrade
wget https://dl.google.com/go/go1.17.2.linux-amd64.tar.gz
sudo tar -xvf go1.17.2.linux-amd64.tar.gz
sudo mv go /usr/local
echo "export GOROOT=/usr/local/go" >> ~/.bashrc
echo "export GOPATH=$HOME/go" >> ~/.bashrc
echo "export PATH=$GOPATH/bin:$GOROOT/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
go version
```
- Run command - ```go run main.go```
- Create windows executable - ```GOOS=windows GOARCH=amd64 go build -o myApp-windows.exe``` 
- Create linux executable - ```GOOS=linux GOARCH=amd64 go build -o myApp-linux```

## Notes
- You cannot have unused variables
- 
