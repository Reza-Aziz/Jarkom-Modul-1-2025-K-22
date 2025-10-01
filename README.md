# Laporan Resmi Praktikum Komunikasi Data dan Jaringan Komputer
1. Membuat topologi jaringan
2. Menyambungkan node eru ke koneksi internet
3. Menghubungkan antar node client
4. Menyambungkan client ainur ke koneksi internet
5. Menginput script ke /root/.bashrc.
* Pada node eru
<pre>
   apt update && apt install iptables -y
   #!/bin/bash
   echo "=== Configuring ERU (Router) ==="
   iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.222.0.0/16
   echo 1 > /proc/sys/net/ipv4/ip_forward
   echo "Router configured!"
</pre>

  * Pada node client
<pre>
   #!/bin/bash
   echo "=== Configuring Client: $(hostname) ==="
   echo nameserver 192.168.122.1 > /etc/resolv.conf
   echo "Client $(hostname) configured!"
</pre>
6.  Execute file traffic.sh dan packet sniffing di wireshark
   * Download, unzip file, dan execute
     <pre>
        apt update && apt install unzip
        wget --no-check-certificate "https://docs.google.com/uc?export=download&id=1bE3kF1Nclw0VyKq4bL2VtOOt53IC7lG5" -O traffic.zip && unzip traffic.zip
        ./traffic.sh
     </pre>
7. Membuat dan konfigurasi FTP server pada node eru, dan buat 2 user baru.
   * install ftp server services
   <pre>
   apt update
   apt install vsftpd
   </pre>

   * buat user dgn hak akses berbeda
   <pre>
   useradd -m -s /bin/bash ainur
   echo "ainur:password123" | chpasswd
   useradd -m -s /bin/bash melkor
   echo "melkor:password456" | chpasswd
   mkdir -p /shared
   chmod 755 /shared
   </pre>

   * edit config ftp
   <pre>
   cp /etc/vsftpd.conf /etc/vsftpd.conf.backup
   nano /etc/vsftpd.conf > /dev/null                                                  
   </pre>
   <pre>
   listen=YES
   anonymous_enable=NO
   local_enable=YES
   write_enable=YES
   local_umask=022
   dirmessage_enable=YES
   use_localtime=YES
   xferlog_enable=YES
   connect_from_port_20=YES
   chroot_local_user=YES
   local_root=/
   allow_writeable_chroot=YES
   secure_chroot_dir=/var/run/vsftpd/empty
   pam_service_name=vsftpd
   pasv_enable=YES
   pasv_min_port=21100
   pasv_max_port=21110
   userlist_enable=YES
   userlist_file=/etc/vsftpd.userlist
   userlist_deny=NO
   </pre>

    * setup use permission
   <pre>
   echo "ainur" > /etc/vsftpd.userlist
   chown ainur:ainur /shared
   chmod 755 /shared
   mkdir -p /etc/vsftpd/user_conf
   echo "local_root=/home/melkor" > /etc/vsftpd/user_conf/melkor
   echo "user_config_dir=/etc/vsftpd/user_conf" >> /etc/vsftpd.conf                                                 
   </pre>

    * buat test file
   <pre>
   cecho "This is a shared file for Ainur group" > /shared/test.txt
   chown ainur:ainur /shared/test.txt
   chmod 644 /shared/test.txt                                                  
   </pre>

   * Start FTP Service
   <pre>
   service vsftpd start
   service vsftpd status
   netstat -tuln | grep :21                                                  
   </pre>

   * Buat client
   <pre>
   apt update
   apt install netbase                                              
   </pre>

   * testing
   <pre>
   ftp 192.222.1.1
   Name: manwe
   Password: pass123
   ftp> cd /shared
   ftp> ls
   test.txt
   test_file.txt
   ftp> get test_file.txt
   ftp> quit                                               
   </pre>

   

   
8. Melakukan koneksi dari node ulmo ke FTP Server eru
   * download file diluar ftp dan unzip
     <pre>
        wget --no-check-certificate "https://docs.google.com/uc?export=download&id=11ra_yTV_adsPIXeIPMSt0vrxCBZu0r33" -O cuaca.zip && unzip cuaca.zip
     </pre>

     * config ulang pada /etc/vsftpd.conf
       <pre>
   listen=YES
   anonymous_enable=NO
   local_enable=YES
   write_enable=YES
   local_umask=022
   dirmessage_enable=YES
   use_localtime=YES
   xferlog_enable=YES
   connect_from_port_20=YES
   chroot_local_user=YES
   local_root=/
   allow_writeable_chroot=YES
   secure_chroot_dir=/var/run/vsftpd/empty
   pam_service_name=vsftpd
   pasv_enable=YES
   pasv_min_port=21100
   pasv_max_port=21110
   userlist_enable=YES
   userlist_file=/etc/vsftpd.userlist
   userlist_deny=NO
   </pre>

   * command start server
     <pre>
        service vsftpd restart
     </pre>
   * masuk ke ftp server di node ulmo, lalu cd /shared kirim kan file dari luar fp ke ftp
     <pre>
         put cuaca.txt
     </pre>
   
   
9. Download file dari FTP Server eru ke node manwe. dan rubah hak akses
    * download file di node server/eru di direktorti /shared dan unzip
      <pre>
         wget --no-check-certificate "https://docs.google.com/uc?export=download&id=11ua2KgBu3MnHEIjhBnzqqv2RMEiJsILY" -O kitab_penciptaan.zip
      </pre>
    * dinode manwe login ftp, dan cd ke /shared. lalu get
    <pre>
        get kitab_penciptaan.txt
    </pre>

    * Pada node eru rubah mode ainur agar read only
      <pre>
         chmod 555 /home/ainur
      </pre>

   * Testing
10. ping ke server eru melalui melkor
    <pre>
       ping 192.222.1.1 -c 100
    </pre>
12. Buat user baru pada node melkor dan masuk dengan telnet melalui node eru
13. Port scanning node melkor memalui node eru dengan netcat
14. Menerapkan koneksi SSH dari node varda ke eru
15.
16.
17.
18.
19.
20.
   .
