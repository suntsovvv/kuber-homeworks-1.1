# Домашнее задание к занятию «Kubernetes. Причины появления. Команда kubectl»   
Установил MicroK8S на удалённую виртуальную машину.   
Установил dashboard.   
Сгенерировал сертификат для подключения к внешнему ip-адресу.   

Для интереса установил kubectl на машине с ОС Windows.   
Настроил подкллючение и получил доступ:   
![image](https://github.com/user-attachments/assets/05168c46-32a7-4698-9704-c37de05ecfc3)  
Создал Service Account и ClusterRoleBinding в dashboard-adminuser.yaml:   
```yml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```
Применил его:   
```bash
user@microk8s:~/.kube$ kubectl apply -f dashboard-adminuser.yaml
serviceaccount/admin-user created
```
Создал токен для подключения:   

```bash
user@microk8s:~/.kube$ kubectl -n kubernetes-dashboard create token admin-user
```

Пробросил порт для подключения к dashboard: 
```bash
user@microk8s:~$ kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443 --address 0.0.0.0
Forwarding from 0.0.0.0:10443 -> 8443
Handling connection for 10443
Handling connection for 10443
Handling connection for 10443
```
Подключился к dashboard:   
![image](https://github.com/user-attachments/assets/b6e78500-d188-4be8-b32f-663bcdcdd21b)

