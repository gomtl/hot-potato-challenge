The Hot Potatoe Challenge - A Go and Kubernetes Hacknight
=========================================================

You're invited to come hack together a network of micro-services. We will:

* Build a micro-service from scratch, however you want.
  * Push it to Docker hub (create an account if needed)
* You'll be given keys to a Kubernetes cluster ([install kubectl from here](https://coreos.com/kubernetes/docs/latest/configure-kubectl.html)).
  * Deploy to Kubernetes (using the sample `service.yaml` and `deployment.yaml` in this repo).
    * Use `kubectl create -f service.yaml -f deployment.yaml`
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


Kubernetes configuration
------------------------

Download the `kube-potatoe` file from the organizer. Place it somewhere safe. Run:

    export KUBECONFIG=path/to/kube-potatoe

You're all set.


To the organizer
----------------

Start a GCP Kubernetes clusetr, the size you want.

Then:

    export CLOUDSDK_CONTAINER_USE_CLIENT_CERTIFICATE=True
    KUBECONFIG=/tmp/kube-potatoe gcloud container clusters get-credentials [NAME OF YOUR CLUSTER] --zone [ZONE] --project [PROJECT NAME]

(just prefix `KUBECONFIG=/tmp/kube-potatoe` to the command GCP gives you).

Share the `/tmp/kube-potatoe` file with participants.

You can share your local directory with:

    python3 -m http.server

Find your IP and ask people to:

    curl -O http://[your-ip]:8000/kube-potatoe
