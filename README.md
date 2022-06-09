# Arsitektur Jaringan Terkini
---

# Tugas 1
## Buat EC2 Instance di AWS Academy

1. Membuka dan menghubuungkan ke [AWS Academy Learner Lab](https://awsacademy.instructure.com/)
    ![Tampilan awal](https://github.com/gnav404/AJT/blob/main/tugas%201/tampilan%20awal.png)
    
2. Buka AWS Management Console untuk membuat sebuah Instance dengan memilih EC2
    ![Tampilan AWS Management Console](https://github.com/gnav404/AJT/blob/main/tugas%201/pilih%20ec2.png?raw=true)
    
4. Membuat Instance baru
    ![Instance Baru](https://github.com/gnav404/AJT/blob/main/tugas%201/instanec%20sudah%20terbuat.png?raw=true)

5. Salin SSH Client untuk menghubungkan AWS dengan Instance yang sudah dibuat
    ![SSH Client](https://github.com/gnav404/AJT/blob/main/tugas%201/copy%20ssh%20id.png?raw=true)

6 Kembali ke laman AWS Academy dan mulai terhubung ke Instance
    ![Menghubungkan](https://github.com/gnav404/AJT/blob/main/tugas%201/menghubungkan%20ke%20ssh.png?raw=true)

7. Setelah terhubung, install Flowmanager, Ryu, dan Mininet
    ![Instalasi selesai](https://github.com/gnav404/AJT/blob/main/tugas%201/menginstall%20beberapa.png?raw=true)


---

# Tugas 2
## Buat Custom Topology Mininet 
![Topologi yang akan dibuat](https://github.com/gnav404/AJT/blob/main/tugas%202/tugas%202.png)

1. Membuat Kode Program untuk sebuah topologi dengan nama `coba.py`

```
from mininet.log import setLogLevel, info
from mininet.topo import Topo

class MyTopo(Topo):
    def addSwitch(self, name, **opts):
        kwargs = {'protocols':'OpenFlow13'}
        kwargs.update(opts)
        return super(MyTopo, self).addSwitch(name, **kwargs)

    def __init__(self):
        #membuat topo
        Topo.__init__(self)

        #membuat switch
        info('*** Add switch\n')
        s1 = self.addSwitch('s1')
        s2 = self.addSwitch('s2')
        s3 = self.addSwitch('s3')

        #membuat host
        info('*** Add hosts\n')
        h1 = self.addHost('h1', ip='10.1.0.1/8')
        h2 = self.addHost('h2', ip='10.1.0.2/8')
        h3 = self.addHost('h3', ip='10.2.0.3/8')
        h4 = self.addHost('h4', ip='10.2.0.4/8’)
        h5 = self.addHost('h5', ip='10.3.0.5/8')
        h6 = self.addHost('h6', ip='10.3.0.6/8')

        #membuat link hots to switch
        info('*** Add Links\n')
        self.addLink(h4, s2, port1=3, port2=1)
        self.addLink(h3, s2, port1=4, port2=1)
        self.addLink(h1, s1, port1=3, port2=1)
        self.addLink(h2, s1, port1=4, port2=1)
        self.addLink(h6, s3, port1=4, port2=1)
        self.addLink(h5, s3, port1=3, port2=1)


        #switch to switch
        self.addLink(s1, s2)
        self.addLink(s2, s3)
        self.addLink(s3, s1)
topos = {'mytopo':(lambda : MyTopo())}

```
2. Sambungkan AWS Academy dengan Instance
3. Setelah berhasil terhubung, upload kode program yang telah dibuat kedalam folder Mininet/custom dengan perintah

    ``` cd ~Mininet/custom ```
    
4. Jika file berhasil diupload, jalankan kode program dengan menggunakan perintah

    ``` sudo mn --controller=none --custom coba.py --topo mytopo --mac --arp ```
   
   Maka akan muncul topologi yang berhasil dibuat
   ![Topologi berhasil dibuat](https://github.com/gnav404/AJT/blob/main/tugas%202/topologi%20dijalankan.png?raw=true)
   
---
