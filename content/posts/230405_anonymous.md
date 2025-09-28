---
title: "Anonymous Browsing"
description: "Running TOR"
author: "Brent Stewart"
date: "2023-04-05T22:04:50-04:00"
math: false
draft: false
Victor_Hugo: "true"
picture: "how to"
Focus_Keyword: "Anonymous Browsing"
youtube: ""
github: ""
refs: ["https://amiunique.org/","https://tails.boum.org/index.en.html","https://www.browserling.com/"]
tags:
  - "Basics"
  - "How To"
  - "Linux"
  - "Virtualization"
  - "Security"
---

What is the best way to be truly anonymous online?  Most of the time our concern is about being commercially tracked and having our browsing habits shared (I understand this doesn't creep everyone out as much as it does me).  My kids think I'm paranoid.  Just for fun, take a look at [Am I Unique](https://amiunique.org) and browse through all the different data points that are shared by your browser and can be used to cross-reference and track you.  Even paranoid people have enemies.
![Am I unique?](/230407_amiunique.png#floatleft)

My personal summary from __Am I Unique?__ is shown to the left.  Just like Mom said, I'm one in a million.

Sometimes our need for anonymity goes beyond shielding our selves from spam.  When the stakes are higher, revealing an identity could impact a job or put lives in danger.  Many people depend on this type of anonymity to circumvent hostile governments or to leak important stories to reporters.

## Recipe for Anonymity
Here's a quick description of how you might approach trying to safeguard your identity.  This will guide you through installing Tails.  Tails is a standalone operating system that allows browsing through TOR.  The machine is clean, so nothing leaks to identify you, the OS is scrubbed every time you boot, the browser is locked down, and TOR passes your traffic through a series of other computers ("nodes") to obscure your source address.

{{<danger title="Danger">}}
This kind of concern means that the authorities are either unconcerned or hostile to your situation.  I am praying for you!

Don't trust my advice!  I have a reasonable level of expertise, but I don't do this for a living.  My advice is a good starting point, but technology is constantly changing and this may be old when you read it.  

I believe Tails by itself is sufficient against non-Nation-State actors.  Especially if you have that level of concern, maintain a sense of paranoia and protect yourself in multiple layers.  For instance, use this post as a starting point but load the USB stick from a computer in a random location that can't be associated to you.  In all cases, continue to research best practice!
{{</danger>}}

Tails can be installed via USB.  If you are in serious danger, this is the best way.  You can then take the USB to any computer, boot up from the USB, and further obscure your source.
{{<danger title="Danger">}}
Tails doesn't protect against stupidity.  Don't login anywhere or take other actions that may identify you while using Tails.  Also, remember that browser crumbs are only one way that someone could be identified.  Cameras, visitor logs, and phone GPS are examples of other ways you could be tied to a location at a specific time.
{{</danger>}}

### Setting up Tails
In this exercise, I'm going to walk through installing Tails in a VM on Linux, which may be sufficient for run-of-the-mill situations.

First, install KVM.  The commands below apply for debian-derived systems like Ubuntu, Mint, or Pop!_OS.

    sudo apt install qemu-kvm libvirt-daemon-system
    sudo adduser $USER libvirt
    sudo apt install virt-manager

Next, download the [Tails ISO](https://tails.boum.org/install/dvd/index.en.html) or an [IMG file](https://tails.boum.org/install/download/) which is easier to write to USB.  I'd use [Etcher](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwio88TRvZr-AhVXIUQIHej7AqwQFnoECCAQAQ&url=https%3A%2F%2Fwww.balena.io%2Fetcher&usg=AOvVaw0UNPm_qcksmQ1aL8D-5gLD) to write to disk, but if you haven't done that part before the Tails website has installation [instructions](https://tails.boum.org/install/index.en.html). 

If you want to install on a VM, run the Virtual Machine Manager and create a new machine.  The device I setup had the following settings:
* Generic Linux 2020
* 2 vCPU, 8 GB RAM (I think you just need 4)
* Boot options - boot from SATA CDROM
* SATA CDROM - mapped to the Tails ISO in my download directory

Start the VM.  The VM window will "capture" your mouse and keyboard - to break out and back to your host machine press the right Win and Ctrl keys.  It will boot to a welcome.  You may want to set some additional parameters (these will only apply until the next boot).

![Additional Settings](/230407_tailsadditionalsettings.png#floatsmallright)

Under Additional settings, choose the plus and set an Administrative password.  This only applies to the session.  The "Unsafe Browser" option allows you to sign into a captive portal.  Again, that's another way to identify you so I would just pick another place to connect and turn that off.

Choose "start Tails" to move into a Gnome desktop.  It will prompt you to start Tor first thing.  You can tell it to connect automatically or "Hide to my local network that I'm connecting to Tor".  The latter choice requires identifying and connecting to a Tor bridge manually.  You'll be prompted to email bridges@torproject.org and they'll help you find a discrete way to connect.

### Specifying Tor Exit Node
One problem that you may encounter is that the site you are trying to contact blocks connections from other countries.  Since Tor will give you a random exit node, there's a good chance your traffic will appear to be coming from somewhere else on the globe.  My first test put me in Germany, for instance.

If you want to force Tor to exit your traffic in a particular country, open a terminal in Tails and edit the tor configuration file.

    sudo nano /etc/tor/torrc

Add these lines to the end of the config file to force traffic to exit in the US.  

    ExitNodes {us}
    StrictNodes 1

I've included a table of exit codes at the end of this document to help you find the appropriate country.

### Browserling
For an extra level of obscurity, use [Browserling](https://www.browserling.com/).  Browserling is a site that will give you a web browser in a Windows VM to test your website.  You can also use this to create another level of difficulty to tracing the connection back.  Use Tor to connect to browserling, which will spin up a temporary VM (it will take a minute).  It's meant for testing web pages and will only be available to you for a couple minutes, so you might want to prep what you want to send in a text editor and then copy and paste it over.  

## Final Note
I hope you don't need this, but if you do I wish you the best.  Hopefully this will give you some ideas where to look and how to protect yourself!

## For Reference -  Tor country codes
{{< bootstrap-table table_class="table table-responsive table-hover" thead_class="table-info" caption="Table of Tor Country Codes" >}}
| Country | Code | Country | Code |
|:-----:|:--:|:-----|:--:|
| Ascension Island | {Ac} | Afghanistan | {Af} |
| Aland | {Ax} | Albania | {Al} |
| Algeria | {Dz} | Andorra | {Ad} |
| Angola | {Ao} | Anguilla | {Ai} |
| Antarctica | {Aq} | Antigua And Barbuda | {Ag} |
| Argentina Republic | {Ar} | Armenia | {Am} |
| Aruba | {Aw} | Australia | {Au} |
| Austria | {At} | Azerbaijan | {Az} |
| Bahamas | {Bs} | Bahrain | {Bh} |
| Bangladesh | {Bd} | Barbados | {Bb} |
| Belarus | {By} | Belgium | {Be} |
| Belize | {Bz} | Benin | {Bj} |
| Bermuda | {Bm} | Bhutan | {Bt} |
| Bolivia | {Bo} | Bosnia And Herzegovina | {Ba} |
| Botswana | {Bw} | Bouvet Island | {Bv} |
| Brazil | {Br} | British Indian Ocean Terr | {Io} |
| British Virgin Islands | {Vg} | Brunei Darussalam | {Bn} |
| Bulgaria | {Bg} | Burkina Faso | {Bf} |
| Burundi | {Bi} | Cambodia | {Kh} |
| Cameroon | {Cm} | Canada | {Ca} |
| Cape Verde | {Cv} | Cayman Islands | {Ky} |
| Central African Republic | {Cf} | Chad | {Td} |
| Chile | {Cl} | People's Republic Of China | {Cn} |
| Christmas Islands | {Cx} | Cocos Islands | {Cc} |
| Colombia | {Co} | Comoras | {Km} |
| Congo | {Cg} | Congo (Democratic Republic) | {Cd} |
| Cook Islands | {Ck} | Costa Rica | {Cr} |
| Cote D Ivoire | {Ci} | Croatia | {Hr} |
| Cuba | {Cu} | Cyprus | {Cy} |
| Czech Republic | {Cz} | Denmark | {Dk} |
| Djibouti | {Dj} | Dominica | {Dm} |
| Dominican Republic | {Do} | East Timor | {Tp} |
| Ecuador | {Ec} | Egypt | {Eg} |
| El Salvador | {Sv} | Equatorial Guinea | {Gq} |
| Estonia | {Ee} | Ethiopia | {Et} |
| Falkland Islands | {Fk} | Faroe Islands | {Fo} |
| Fiji | {Fj} | Finland | {Fi} |
| France | {Fr} | France Metropolitan | {Fx} |
| French Guiana | {Gf} | French Polynesia | {Pf} |
| French Southern Territories | {Tf} | Gabon | {Ga} |
| Gambia | {Gm} | Georgia | {Ge} |
| Germany | {De} | Ghana | {Gh} |
| Gibralter | {Gi} | Greece | {Gr} |
| Greenland | {Gl} | Grenada | {Gd} |
| Guadeloupe | {Gp} | Guam | {Gu} | 
| Guatemala | {Gt} | Guinea | {Gn} |
| Guinea-Bissau | {Gw} | Guyana | {Gy} |
| Haiti | {Ht} | Heard-Mcdonald Island | {Hm} |
| Honduras | {Hn} | Hong Kong | {Hk} |
| Hungary | {Hu} | Iceland | {Is} |
| India | {In} | Indonesia | {Id} |
| Iran, Islamic Republic Of | {Ir} | Iraq | {Iq} |
| Ireland | {Ie} | Isle Of Man | {Im} |
| Israel | {Il} | Italy | {It} |
| Jamaica | {Jm} | Japan | {Jp} |
| Jordan | {Jo} | Kazakhstan | {Kz} |
| Kenya | {Ke} | Kiribati | {Ki} |
| Korea, Dem. Peoples Rep Of | {Kp} | Korea, Republic Of | {Kr} |
| Kuwait | {Kw} | Kyrgyzstan | {Kg} |
| Lao People's Dem. Republic | {La} | Latvia | {Lv} |
| Lebanon | {Lb} | Lesotho | {Ls} |
| Liberia | {Lr} | Libyan Arab Jamahiriya | {Ly} |
| Liechtenstein | {Li} | Lithuania | {Lt} |
| Luxembourg | {Lu} | Macao | {Mo} |
| Macedonia | {Mk} | Madagascar | {Mg} |
| Malawi | {Mw} | Malaysia | {My} |
| Maldives | {Mv} | Mali | {Ml} |
| Malta | {Mt} | Marshall Islands | {Mh} |
| Martinique | {Mq} | Mauritania | {Mr} |
| Mauritius | {Mu} | Mayotte | {Yt} |
| Mexico | {Mx} | Micronesia | {Fm} |
| Moldava Republic Of | {Md} | Monaco | {Mc} |
| Mongolia | {Mn} | Montenegro | {Me} |
| Montserrat | {Ms} | Morocco | {Ma} |
| Mozambique | {Mz} | Myanmar | {Mm} |
| Namibia | {Na} | Nauru | {Nr} |
| Nepal | {Np} | Netherlands Antilles | {An} |
| Netherlands | {Nl} | New Caledonia | {Nc} |
| New Zealand | {Nz} | Nicaragua | {Ni} |
| Niger | {Ne} | Nigeria | {Ng} |
| Niue | {Nu} | Norfolk Island | {Nf} |
| Northern Mariana Islands | {Mp} | Norway | {No} |
| Oman | {Om} | Pakistan | {Pk} |
| Palau | {Pw} | Palestine | {Ps} |
| Panama | {Pa} | Papua New Guinea | {Pg} |
| Paraguay | {Py} | Peru | {Pe} |
| Poland | {Pl} | Portugal | {Pt} |
| Puerto Rico | {Pr} | Qatar | {Qa} |
| Reunion | {Re} | Romania | {Ro} |
| Russian Federation | {Ru} | Rwanda | {Rw} |
| Samoa | {Ws} | San Marino | {Sm} |
| Sao Tome/Principe | {St} | Saudi Arabia | {Sa} |
| Scotland | {Uk} | Senegal | {Sn} |
| Serbia | {Rs} | Seychelles | {Sc} |
| Sierra Leone | {Sl} |Singapore | {Sg} |
| Slovakia | {Sk} | Slovenia | {Si} |
| Solomon Islands | {Sb} | Somalia | {So} |
| Somoa,Gilbert,Ellice Islands | {As} | South Africa | {Za} |
| South Georgia, South Sandwich Islands | {Gs} | Spain | {Es} |
| Sri Lanka | {Lk} | St. Helena | {Sh} |
| St. Kitts And Nevis | {Kn} | St. Lucia | {Lc} |
| St. Pierre And Miquelon | {Pm} | St. Vincent - The Grenadines | {Vc} |
| Sudan | {Sd} | Suriname | {Sr} |
| Svalbard And Jan Mayen | {Sj} | Swaziland | {Sz} |
| Sweden | {Se} | Switzerland | {Ch} |
| Syrian Arab Republic | {Sy} | Taiwan | {Tw} |
| Tajikistan | {Tj} | Tanzania | {Tz} |
| Thailand | {Th} | Togo | {Tg} |
| Tokelau | {Tk} | Tonga | {To} |
| Trinidad And Tobago | {Tt} | Tunisia | {Tn} |
| Turkey | {Tr} | Turkmenistan | {Tm} |
| Turks And Calcos Islands | {Tc} | Tuvalu | {Tv} |
| Uganda | {Ug} | Ukraine | {Ua} | 
| United Arab Emirates | {Ae} | United Kingdom | {Gb} |
| United Kingdom | {Uk} | United States | {Us} |
| United States Minor Outl.Islands | {Um} | Uruguay | {Uy} |
| Uzbekistan | {Uz} | Vanuatu | {Vu} |
| Vatican City State | {Va} | Venezuela | {Ve} |
| Viet Nam | {Vn} | US Virgin Islands | {Vi} |
| Wallis And Futuna Islands | {Wf} | Western Sahara | {Eh} |
| Yemen | {Ye} | Zambia | {Zm} |
| Zimbabwe | {Zw} |
{{</bootstrap-table>}}