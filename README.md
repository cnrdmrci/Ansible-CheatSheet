# Ansible

### Ansible nedir?
Ansible; Bir veya birden çok sunucuya gerekli yazılımların yüklenmesini ve yönetimini sağlayan open source bir uygulamadır. Kurulumu ve kullanımı basittir. Sunuculara ssh ile bağlantı kurarak istenilen işlemleri yerine getirebilmektedir. Kullanılan ssh bağlantısı sayesinde güvenli işlemler gerçekleştirmektedir.

### Kurulum 
Centos makinesine kurulum için aşağıdaki komut çalıştırılabilir. Sadece ansible server olarak kullanılacak makineye kurulum yapılmalıdır.
> sudo  yum install -y ansible

### Sunucular ile iletişim

#### Ping
Ansible kurulduktan sonra; /etc/ansible/hosts dosyası içerisine erişim yapılacak sunucuların adresleri belirtilmelidir. /etc/ansible/hosts içerisinde gruplayarak tutabileceğimiz sunucu adreslerine başarılı erişimin olduğunu test etmek için ansible tarafından, tüm sunuculara ping komutu aşağıdaki şekilde çalıştırılmıştır.
> ansible -m ping all

Sadece belirli bir sunucuya istek atmak için ilgili sunucunun adı yazılabilir.
> ansible -m ping [server name]

#### Dosya kopyalama
Sunuculara dosya kopyalamak için aşağıdaki komut çalıştırılabilir.
> ansible -m copy -a "src=./Test.txt dest=/tmp/Test.txt" all

#### Program yükleme ve silme
Program yüklemek için aşağıdaki komut çalıştırılabilir.
> ansible -m yum -a 'name=vim state=present' all

Yüklenen programı kaldırmak için ise aşağıdaki komut çalıştırılabilir.
> ansible -m yum -a 'name=vim state=absent' all

#### Komut çalıştırma
> ansible -a 'ls' [server name]

> ansible -m shell -a 'ls' [server name]

#### Rol oluşturma
> ansible-galaxy init role1

#### Playbook çalıştırma
> ansible-playbook pb.yml

### Ansible Terminoloji
Ansible server; İşlemlerin yürütüldüğü sunucu.

Inventory; Sunucular hakkında bilgi içeren dosya.

Playbook; İşlemlerin otomatikleştirilmesi durumu için hazırlanan YAML dosyası.

Task; Yapılacak tek bir işlemi tanımlayan parça.

Module; Soyutlanması istenen görevler, ayrı bir kısım olarak ele alınır.

Role; Yeniden kullanımı kolaylaştırmak için belirli işlemlerin önceden hazırlanması durumu.

Play; playbook dosyasının çalıştırılması.

Facts; Değişkenlerin global olarak saklanması.

Handlers; Servisleri başlatmak, yeniden başlatmak ve durdurmak için kullanılır.

### Ansible Playbook
İşlemlerin otomatikleştirilmesi hazırlanacak YAML dosyası detay;

##### Tüm sunuculara vim yükler
```ruby
---
- name: playbook name # genel playbook adı
- hosts: all # ilgili sunucuları belirtir.
  become: true # komutlari yetkili olarak(sudo) calistirir.
  tasks: # ilgili görevler
     - name: Update apt-cache 
       apt: update_cache=yes

     - name: Install Vim
       apt: name=vim state=latest
```

##### Yukarıdaki örnek gibi tüm sunuculara vim yükler
```ruby
---
- hosts: all
  become: true
  vars:
     package: vim
  tasks:
     - name: Install Package
       apt: name={{ package }} state=latest
```

##### master grubundaki sunuculara vim,git,curl yükler
```ruby
---
- hosts: master
  become: true
  tasks:
     - name: Install Packages
       apt: name={{ item }} state=latest
       with_items:
          - vim
          - git
          - curl  
```

##### debug ile hata bilgisi
```ruby
---
- hosts: all
  become: true
  tasks:
     - name: Install Package
       register: vim_install
       apt: name=vim state=present
       ignore_errors: true
       
     - name: vim yuklemesi basarili # bu kisim sadece basarili durumunda calisir.
       debug: var=vim_install
       when: vim_install is success
       
     - name: vim yuklemesi basarisiz #bu kisim sadece basarisiz durumunda calisir.
       debug: msg='vim yuklemesi basarisiz'
       when: vim_install is failed
```

##### servis işlemleri
nginx yükleme ve başlatma işlemi;
```ruby
---
- hosts: all
  become: true
  tasks:
     - name: Install Package
       apt: name=vim state=latest
  handlers:
      - name: start nginx
        service: name=nginx state=started
```
nginx durdurma ve silme işlemi;
```ruby
---
- hosts: all
  become: true
  tasks:
  handlers:
     - name: start nginx
       service:
         name: nginx
         state: stopped
     - name: Remove nginx
       apt: name=nginx state=absent

```
