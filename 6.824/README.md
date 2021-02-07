## Intro
- 6.824 Spring 2021
## Prerequisite
### Setup OS
- Linux env. (eg. Win10 WSL Ubuntu 1803)
### Setup Go
- Install `Go` with version 1.13.6
  - Download and tar: `wget -qO- https://dl.google.com/go/go1.13.6.linux-amd64.tar.gz | sudo tar xz -C /usr/local`
  - Set path env: `export PATH="$PATH:/usr/local/bin"`
  - Copy binary file to path: `cd /usr/local/go/bin && cp -rf * /usr/local/bin`
  - Test env: `go version`
    - `> go version go1.13.6 linux/amd64`

## Lab1 Map Reduce
- [Lab01](https://pdos.csail.mit.edu/6.824/labs/lab-mr.html)
- [Paper](https://pdos.csail.mit.edu/6.824/papers/mapreduce.pdf)

### Paper Brief
![20210207173555](https://raw.githubusercontent.com/zjs1224522500/PicGoImages/master//img/blog/20210207173555.png)

### Original Demo
#### Prepare
- Fetch initial lab with git: `git clone git://g.csail.mit.edu/6.824-golabs-2020 6.824` 

#### Demo Test
- WordCount `wc.go` Input `pg*.txt` Output `mr-out*`
  - `cd ~/6.824/src/main`
  - Build wc.go: `go build -buildmode=plugin ../mrapps/wc.go`
  - Delete previous output: `rm mr-out*`
  - Run wc.go to execute word count task with input file: `go run mrsequential.go wc.so pg*.txt`
  - Check output: `more mr-out-0`

#### Code Reading
##### mrsequential.go
- Simple sequential MapReduce.
- `go run mrsequential.go wc.so pg*.txt`
- The code comments are in the `mrsequential.go`.
- The program checks the args from cmd line and use the plugin which contains functions map and reduce to execute MapReduce tasks.
- The base process: 
```go
func main() {
    ...
    mapf, reducef := loadPlugin(os.Args[1])
    ...
    for _, filename := range os.Args[2:] {
        ...
        kva := mapf(filename, string(content))
        ...
    }

    for i < len(intermediate) {
        ...
    	output := reducef(intermediate[i].Key, values)
        ...
    }
}
```

##### wc.go
- A word-count application "plugin" for MapReduce.
- Build it as a plugin: `go build -buildmode=plugin wc.go`
- The code comments are in the `src/main/mrapps/mrsequential.go`.
- The program defines the function `map` and `reduce`. And the two functions are simple, please check it in src code.

### Job
- Implement a distributed MapReduce, consisting of two programs, the `coordinator` and the `worker`. Please click the [6.824 Lab 1: MapReduce](https://pdos.csail.mit.edu/6.824/labs/lab-mr.html) to view more lab1 details.