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
   * Download dan unzip file
     <pre>
        apt update && apt install unzip
        wget --no-check-certificate "https://docs.google.com/uc?export=download&id=1bE3kF1Nclw0VyKq4bL2VtOOt53IC7lG5" -O traffic.zip && unzip traffic.zip
     </pre>
7. Membuat dan konfigurasi FTP server pada node eru, dan buat 2 user baru.
9. Melakukan koneksi dari node ulmo ke FTP Server eru
10. Download file dari FTP Server eru ke node manwe. dan rubah hak akses
11. ping ke server eru melalui melkor
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
