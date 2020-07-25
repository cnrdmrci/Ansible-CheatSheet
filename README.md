# Ansible

### Ansible nedir?
Ansible; Bir veya birden çok sunucuya gerekli yazılımların yüklenmesini ve yönetimini sağlayan open source bir uygulamadır. Kurulumu ve kullanımı basittir. Sunuculara ssh ile bağlantı kurarak istenilen işlemleri yerine getirebilmektedir. Kullanılan ssh bağlantısı sayesinde güvenli işlemler gerçekleştirmektedir.

### Kurulum 
Centos makinesine kurulum için aşağıdaki komut çalıştırılabilir. Sadece ansible server olarak kullanılacak makineye kurulum yapılmalıdır.
> sudo  yum install -y ansible

Ansible kurulduktan sonra; /etc/ansible/hosts dosyası içerisine erişim yapılacak sunucuların adresleri belirtilmelidir. /etc/ansible/hosts içerisinde gruplayarak tutabileceğimiz sunucu adreslerine başarılı erişimin olduğunu test etmek için ansible tarafından, tüm sunuculara ping komutu aşağıdaki şekilde çalıştırılmıştır.
> ansible -m ping all
