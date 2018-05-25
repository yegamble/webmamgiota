# webmamgiota

<span style="color:red">WARNING: DO NOT USE THE SEED OF YOUR WALLET!</span> 

NOTE: Tab `Check for IoT data` not working yet. Work in progress.
NOTE: You must fill in a number (0 is fine) in the Value IOTAs field, otherwise the app will panic.

This web-app is still under construction and no safety tests have been conducted yet. If you want to, only use small IOTA values, and do not use our wallet seed. Otherwise and even better, use https://nodes.testnet.thetangle.org:443 and later Spamnet at https://nodes.spamnet.iota.org/ (Previous testnet) for testing purposes,with a MWM of 9 to 14 as is suggested here https://blog.iota.org/first-of-the-new-testnets-live-f8f41b99e9a3 I've set mwm to 9 which is fast.

`webmamgiota` is a small project to implement Masked Authenticated Messaging on the IOTA tangle with Golang via a web UI.

This is work in progress and still under construction (see TODO) with the aim to get IoT sensors and devices to send MAMs. No Merkle Tree is implemented yet, which 
is a priority for now.

## Install

It is assumed that you have Golang installed. You also need to install the Go library API for IOTA which you can download at:

```go
go get -u github.com/iotaledger/giota
```

After that you can download the webmamgiota package.

```go
go get -u github.com/habpygo/webmamgiota
```
It is assumed that in will be installed in your `$GOPATH/src` otherwise you will have to vendor it yourself.
## Sending MAMs to the IOTA tangle with Go

### API

#### Run the web-app

In the root directory enter `go run main.go`. Your browser automatically opens up on the Send message page. 

#### Connection
A new connection is automatically created when the app is started and pointed to port 3000 of your local webbrowser.

```go
func main() {
	...
	open("http://localhost:3000/")
	web.Serve(msgwebpage)
}
```

The node currently defauls to `http://node02.iotatoken.nl:14265` but any 
nodeURL works.
If you don't have a nodeURL try out one from: http://iotasupport.com/lightwallet.shtml

If you don't have a seed yet, follow the description here: https://iota.readme.io/docs/securely-generating-a-seed

Please keep in mind that you may NEVER loose this seed nor give it to anybody else, because the seed is the connection to your funds!




#### Send a MAM to the IOTA tangle

After the webpage has opened, you can write a message in the input field labeled "Text message". Fill in 0 for Value (not working yet) and press the `Send message` button. This might take a while depending on the traffic.
After sending, you find your transaction by clicking on the `Query address for all messages`.

You can also peruse it here https://thetangle.org giving the TransactionId.

<!-- If you want to transfer value aswell (here 100 IOTA) call the send method like this: ```Send("the receiving address", 100, "your stringified message", c)```. -->

#### Read data from the IOTA tangle from the CLI

Reading all transaction received by a certain adress:
```go
import "github.com/iotaledger/mamgoiota"

func main(){
    c, err := NewConnection("someNodeURL", "")
    if err != nil{
        panic(err)
    }

    ts, err := ReadTransactions("Receiving Address", c)
    if err != nil{
        panic(err)
    }
    for i, tr := range ts {
        t.Logf("%d. %v: %d IOTA, %v to %v\n", i+1, tr.Timestamp, tr.Value, tr.Message, tr.Recipient)
    }
}
```
The seed can be ommitted here, since reading does not require an account



Reading a special transaction by transactionID:
```go
import "github.com/iotaledger/mamgoiota"

func main(){
    c, err := NewConnection("someNodeURL", "")
    if err != nil{
        panic(err)
    }

    tx, err := ReadTransaction("Some transactionID", c)
    if err != nil{
        panic(err)
    }
    t.Logf("%v: %d IOTA, %v to %v\n", tx.Timestamp, tx.Value, tx.Message, tx.Recipient)
}
```

#### Examples webmamgiota/mamgoiota

These examples won't work anymore on this site. Hopefully we will manage to get this workin with the `iotaledger/iota.lib.go` repository on GitHub.

Check out our [example folder](/example) for a send and a receive example.

To run this, cd into the example folder and edit the `sender/send.go` and `receiver/receive.go` file, set the correct provider and address and you are ready to run.

Start the receiver first: `$ go run receiver/receive.go`. It will check for new messages every 5 seconds, until cancelled.

Then start the sender: `$ go run sender/send.go`.

You can also read all the past transactions, i.e. messages + value,  at the address: `go run history/history.go`.

If you pick up the transaction hash from the Terminal output and paste it into the input field on the site https://thetangle.org you find your transaction.

If the Node is offline try another one, mentioned above.
Most TODO'a are similar to the `mam.client.go`.
### TODOs

- [ ] Make use of Merkle Tree (binary) to make proper masked authenticated messages, a priority for now
- [ ] Still receiving Travis errors
- [ ] GoDoc
- [ ] Read sensor data, e.g. RuuVi tag
- [ ] More Read options
- [ ] Send Value
- [X] Read by TransactionId





