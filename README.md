# Automatic-Operations-with-Python-and-Selenium

from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

print("https://www.linkedin.com/in/yusuf-%C3%A7etinkaya-488615198/")

# Site Url, Kullanıcı e-posta ve şifre bilgileri
site_linki = input("Site Linki: ")
email = input("E-posta adresinizi girin: ")
password = input("Şifrenizi girin: ")

#Tarayıcı driver yolu
service = Service("./chromedriver.exe")
driver = webdriver.Chrome(service=service)

# Tarayıcıyı tam ekran
driver.maximize_window()
driver.get(site_linki)
#Sayfa yüklenmesini beklemesi için
wait = WebDriverWait(driver, 10)

#Video Sayfası
link = wait.until(EC.element_to_be_clickable((By.LINK_TEXT, "Kariyer Planlama Dersi")))
link.click()

# Giriş yap butonunu bul ve tıkla
girisyap = driver.find_element(By.CLASS_NAME, "btnGiris")
girisyap.click()

# Explicit wait kullanarak e-posta ve şifre alanlarının görünür olmasını
eposta = wait.until(EC.visibility_of_element_located((By.ID, "TxtEposta")))
sifre = wait.until(EC.visibility_of_element_located((By.ID, "TxtSifre")))
# E-posta ve şifre girişi
eposta.send_keys(email)
sifre.send_keys(password)
#Login işlemi
login_butonu = wait.until(EC.element_to_be_clickable((By.CLASS_NAME, "btn-primary")))
login_butonu.click()

#Video kaldığı yerden devam etmesi
devam_et_link = wait.until(EC.element_to_be_clickable((By.CSS_SELECTOR, "a.btn.btn-outline-primary.p-2")))
devam_et_link.click()

while True:
    # Video durumunu kontrol etme ve gerektiğinde tekrar oynat
    video_status = driver.execute_script("return document.querySelector('video').paused")
    if video_status:
        driver.execute_script("document.querySelector('.vjs-big-play-button').click()")

    # Sonraki Konu butonunun varlığını kontrol etme
    sonraki_konu_button = driver.find_elements(By.CLASS_NAME, "swal-button--nextone")
    if sonraki_konu_button:
        # Sonraki Konu butonuna tıklama
        sonraki_konu_button[0].click()

# Kullanıcı girişi tamamlandıktan sonra bir çıktı yazdırın
print("Giriş başarılı. Programı kapatmak için Enter tuşuna basın...")
# Kullanıcının programı kapatmasını bekleyin
input()
