# Install InfluxDB on Ubuntu Server

1. Download link

   [https://www.influxdata.com/downloads/](https://www.influxdata.com/downloads/)

   Or use the commands blew:

   - `wget -q https://repos.influxdata.com/influxdata-archive_compat.key`
   - `echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c && cat influxdata-archive_compat.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg > /dev/null`
   - `echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list`
   - `sudo apt-get update`
   - `sudo apt-get install influxdb2`
1. `influx version`
2. `sudo systemctl start influxdb`
3. Enable InfluxDB as a service

   `sudo systemctl enable influxdb`

1. `sudo service influxdb status`
2. Allow InfluxDB TCP port 8086 in the Firewall

   `sudo ufw allow 8086/tcp`

1. WebUI

   [http://localhost:8086](http://localhost:8086)

