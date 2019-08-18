## Apply yml defination to kubernetes

    kubectl apply -f <file_name.yml> -f <namespace>

## Delete all kubernetes objects defined in an yaml file

    kubectl delete -f <file_name.yml> -f <namespace>

## List all kubernetes objects in the namespace
    
    kubectl get all -n <namespace>

## List all pods
   
    kubectl get pods -n <namespace>

## List Network policy

    kubectl get netpol -n <namespace>

## Delete any kubernetes object
  
    kubectl delete -n <namespace> <object_type>/<object_name> ...

    for example :

    kubectl delete -n <namespace> pod/nginx-wdswe2-343dcd service/nginx