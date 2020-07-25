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

#### Rol oluşturma
> ansible-galaxy init role1

#### Playbook çalıştırma
> ansible-playbook pb.yml
