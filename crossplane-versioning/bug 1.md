There seems to be a race condition.



Claims likely to stay as "synced false":

kubectl apply -f 1.yaml
sleep 5
kubectl apply -f 2.yaml
sleep 5
kubectl apply -f 3.yaml
sleep 5
kubectl apply -f 4.yaml
sleep 5
kubectl apply -f 5.yaml
sleep 5
kubectl get claim
kubectl patch xrd xsimples.example.guidewire.net --type='json' -p='[{"op": "replace", "path": "/spec/versions/0/referenceable", "value":false},{"op": "replace", "path": "/spec/versions/1/referenceable", "value":true}]'
sleep 5
kubectl get claim




Claims likely to stay as "synced true":

kubectl apply -f 1.yaml
sleep 5
kubectl apply -f 2.yaml
sleep 5
kubectl apply -f 3.yaml
sleep 5
kubectl apply -f 4.yaml
#sleep 5
kubectl apply -f 5.yaml
#sleep 5
kubectl get claim
kubectl patch xrd xsimples.example.guidewire.net --type='json' -p='[{"op": "replace", "path": "/spec/versions/0/referenceable", "value":false},{"op": "replace", "path": "/spec/versions/1/referenceable", "value":true}]'
sleep 5
kubectl get claim




Reset:

kubectl delete --timeout=1s `kubectl get claim -o name`
kubectl delete `kubectl get composite -o name`
kubectl delete `kubectl get composition -o name`
kubectl delete `kubectl get xrd -o name`

sleep 10

kubectl get claim
kubectl get composite
kubectl get composition
kubectl get xrd


