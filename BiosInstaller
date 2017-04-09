from robobrowser import RoboBrowser
from bs4 import BeautifulSoup
import requests, warnings, sys, os, io, subprocess, time
warnings.filterwarnings("ignore")

try:
#Initialisierung, Computermodell wird ausgelesen, Browser aufgesetzt
	Input = subprocess.Popen("wmic computersystem get model", stdout=subprocess.PIPE).stdout.read()
	Input = Input.decode().split('\n')[1]
	Input = "".join(Input.split('\r'))


	browser = RoboBrowser(
	history=True,
	user_agent='Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:42.0) Gecko/20100101 Chromium/42.0',
	)

#Alle Links werden aus der dell FTP Seite ausgelesen
	browser.open('http://downloads.dell.com/published/Pages/index.html')
	Links=browser.find_all(href=True)

#Und abgeglichen mit dem Namen des eingegebenen Rechners (der auf der Seite sichtbare Name des Links)
#Dabei werden Leerzeichen entfernt und alles auf Kleinbuchstaben reduziert um Fehler zu vermeiden
	for Link in Links:
		if Input.lower().replace(" ", "") in Link.get_text().lower().replace(" ", ""):
		#Der gefundene Link wird geöffnet, die weitere Suche abgebrochen
			browser.open('http://downloads.dell.com/published/Pages/' + Link.get('href'))
			break

#Auf der neuen Seite wird nach der Tabelle mit der BIOS-id gesucht und vermerkt		
	BiosTable = browser.find_all(id='Drivers-Category.BI-Type.BIOS', limit=1)
	BiosTable = BiosTable[0]

#damit aus dieser dann der 2. Link (der 1., also aktuellste Downloadlink) ausgelesen wird
	BiosDownloads = BiosTable.find_all('a', limit=2)
	curDownload = BiosDownloads[1]
	curDownload = 'http://downloads.dell.com/' + curDownload['href']

#Der Download wird via request gestartet und ins Verzeichnis des Skripts geschrieben, Name lautet Bios.exe
	DL = os.path.dirname(os.path.realpath(sys.argv[0])) + '\Bios.exe'
	r = requests.get(curDownload)
	with io.open(DL, 'wb') as file:
		file.write(r.content)

#Hier wird zuletzt ein Powershell snippet losgelassen, das mit sendkeys die Biosinstallation selbständig vollzieht
	subprocess.Popen(DL)
	time.sleep(1)
#	subprocess.Popen(Powershellprefix(ipconfig; ipconfig))
	time.sleep(0.1)

except Exception as ex:
	print(ex)
	input("Press Enter to continue...")
	
#Es fehlen: keystrokes, die Eigenständig die Installation vollziehen
