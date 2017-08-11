The Hot Potatoe Challenge - A Go and Kubernetes Hacknight
=========================================================

You're invited to come hack together a network of micro-services. We will:

* Build a micro-service from scratch, however you want.
* You'll be given keys to a Kubernetes cluster ([https://coreos.com/kubernetes/docs/latest/configure-kubectl.html](install kubectl from here)).
* We'll network all our micro-services:
  * Your service will call other folks's services (implementing the API below)
    creating a ginormous pipeline of text processing.
    * Ex: my process upper-cases its inputs, your process maps each word to a Tweet, etc..
* We'll see what gets created by calling each service.


API to implement
----------------

`POST /process`

```
{"text": "text",
 "history": [
    {"node": "user1", "text": "hello-world", "desc": "We upper-cased the hell out of it"},
  ]
}
```

Should output:

```
{"text": "new_text",
 "history": [
    {"node": "user1", "text": "hello-world"},
    {"node": "latest-call", "text": "new_text"},
 ]}
```

Always add your "text" reply to the history before returning, along
with your `hostname` (or some other debug messages, for tracing).


Golang base code
----------------

type Potatoe struct {
    Text    string  `json:"text"`
    History []Entry `json:"history"`
}

type Entry struct {
    Node string `json:"node"`
    Text string `json:"text"`
    Desc string `json:"desc"`
}
