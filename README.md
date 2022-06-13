1. Попробуйте запустить playbook на окружении из test.yml, зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a.


    lianast11@Liudmilas-MBP ansible % ansible-playbook site.yml -i inventory/test.yml

    "msg": "12"

2. Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.

    some_fact: 'all default fact'

3. Воспользуйтесь подготовленным (используется docker) или создайте собственное окружение для проведения дальнейших испытаний.
   
    lianast11@Liudmilas-MBP playbook % docker pull pycontribs/ubuntu 

    lianast11@Liudmilas-MBP playbook % docker run --name ubuntu -d pycontribs/ubuntu sleep 60000000000

    lianast11@Liudmilas-MBP playbook % docker pull pycontribs/centos7

    lianast11@Liudmilas-MBP playbook % docker run --name centos7 -d pycontribs/centos7 sleep 60000000000

4. Проведите запуск playbook на окружении из prod.yml. Зафиксируйте полученные значения some_fact для каждого из managed host.
   
    lianast11@Liudmilas-MBP playbook % ansible-playbook site.yml -i inventory/prod.yml

```TASK [Print fact] ***************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}
```

5. Добавьте факты в group_vars каждой из групп хостов так, чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.

6. Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.
   
```ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
```

7. При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.
   
    lianast11@Liudmilas-MBP playbook % ansible-vault encrypt group_vars/deb/examp.yml

    lianast11@Liudmilas-MBP playbook % ansible-vault encrypt group_vars/el/examp.yml

8. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.
   
    lianast11@Liudmilas-MBP playbook % ansible-playbook site.yml -i inventory/prod.yml --ask-vault-pass 

9.  Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.

    lianast11@Liudmilas-MBP playbook % ansible-doc -l  

    lianast11@Liudmilas-MBP playbook % ansible-doc plugin command 

10. В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения.

```    
local:
    hosts:
      localhost:
        ansible_connection: local
```

11. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.

```    
TASK [Print fact] ***************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [localhost] => {
    "msg": "all default fact"
}
```

12. Заполните README.md ответами на вопросы. Сделайте git push в ветку master. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым playbook и заполненным README.md .
    

