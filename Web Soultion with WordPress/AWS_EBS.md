AWS EBS " Elastic Block Store"

Link:
https://www.youtube.com/watch?v=HPXnXkBzIHw

```
lsblk
```

```
file -s /dev/xvd
```


```
file -s /devv/xvd
```



```
file -s /dev/xvdf
```

```
mkfs -t ext4 /dev/xvdf
```

![image](https://github.com/user-attachments/assets/32debf7d-34dc-469a-bc14-ba98a1b3ebe7)





```
mkdir /newstorage
```

```
mount /dev/xvdf/newStorage/
```

```
df -hT
```




```
vi /etc/fstab
```
and copy past aat lat
```
/dev/xvdf /newStorage ext4 defaults,nofail, 0 0
```

![image](https://github.com/user-attachments/assets/ea5a7ac2-b2d9-480b-8c5e-172cf23b1345)

![image](https://github.com/user-attachments/assets/8ce1a7a0-2e5a-4b06-91a2-a5d168d2b47e)


```
mount -a
```

```
df -hT
```
![image](https://github.com/user-attachments/assets/7b8d2a76-559b-4651-8b77-6f2b584c8f28)

 





