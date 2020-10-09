### Install
sudo apt install tftpd-hpa

### Check status 
sudo systemctl status tftpd-hpa

### Configuring TFTP Server:
$ sudo vim /etc/default/tftpd-hpa

Here, TFTP_USERNAME is set to tftp. It means the TFTP server will run as the user tftp.

TFTP_DIRECTORY is set to /var/lib/tftpboot. It means /var/lib/tftpboot is the directory on this server which you will be able to accessing via TFTP.

TFTP_ADDRESS is set to :69. It means TFTP will run on port 69.

TFTP_OPTIONS is set to –secure. This variable sets the TFTP options. There are many options that you can use to configure how the TFTP server will behave. I will talk about some of them later. The –secure option means change the TFTP directory to what is set on the TFTP_DIRECTORY variable when you connect to the TFTP server automatically. 

### restart the tftpd-hpa service
sudo systemctl restart tftpd-hpa

source:
https://linuxhint.com/install_tftp_server_ubuntu/
