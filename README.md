# sample-crd-dep-svc-ing

### create cluster to run ingress controller

<pre>
 $ kind create cluster --config clust.yaml
</pre>

### add ingress controller
<pre>
 $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
</pre>

# Crd

### inital file structure 
<pre>
 |
 |-pkg
 |    |-apis
 |          |-xapi.com
 |                    |-v1
 |                    |   |-doc.go
 |                    |   |-register.go
 |                    |   |-types.go
 |                    |-register.go
 |_hack
 |     |-update_codegen.sh
 |-vendor...
 
  1. change the files according to the neccessity : 1. types.go [make the structures and functions; add codegen notations for deepcopy]
                                                    2. update_codegen.sh [set the directories according the file structure]
                                                    3. doc.go [add codegen notations]
  2. give permission : $ chmod +x vendor/k8s.io/code-generator/generate-groups.sh
                       $ chmod +x hack/update_codegen.sh
</pre>

### generate deepcopy 
<pre>
  $ ./hack/update_codegen.sh
</pre>

### generate sample yaml
<pre>
  $ controller-gen rbac:roleName=xt crd paths=./... output:crd:dir=sampleYaml output:stdout
</pre>

### introduce your crd to the cluster
<pre>
  $ kubectl apply -f sampleYaml/xapi.com_xcrds.yaml
</pre>

# Controller

### write controller
<pre>
  1. add file controller.go
  2. add file main.go
  3. edit all the files accordingly
</pre>

### ready to go
<pre>
  1. write test.yaml
  2. $ sudo nano /etc/hosts 
          add this -> 127.0.0.1   tasdid2.com
  2. $ kubectl apply -f test.yaml
  3. $ go build -o app .
  4. $ ./app
  5. enjoy the show by hitting any of the urls [ tasdid2.com/hi ; tasdid2.com/hello ; tasdid2.com/bye ] from your chrome 
</pre>
