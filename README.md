using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
using System.Threading.Tasks;
using Microsoft.Win32;
using System.Diagnostics;
using System.Collections;
using System.Net;
using System.IO;
using System.Net.NetworkInformation;
using System.Reflection;
using Microsoft.Win32;
using System.Management;
using System.Linq;
using System.Web;
using System.Xml;
namespace BilgisayarBilgiler
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            NetworkInterface[] nics = NetworkInterface.GetAllNetworkInterfaces();
            String sMacAddress = string.Empty;
            foreach (NetworkInterface adapter in nics)
            {
                if (sMacAddress == String.Empty) sMacAddress = adapter.GetPhysicalAddress().ToString();
                label2.Text = "MAC ADRESİNİZ: " + " " + (sMacAddress);
            }
            ManagementObjectSearcher Search = new ManagementObjectSearcher("Select * From Win32_ComputerSystem");
            foreach (ManagementObject Mobject in Search.Get())
            {
                double Ram_Bytes = (Convert.ToDouble(Mobject["TotalPhysicalMemory"]));
                double ramgb = Ram_Bytes / 1073741824;
                double islem = Math.Ceiling(ramgb);
                label3.Text = "RAM MİKTARINIZ: " + " " + islem.ToString() + " GB";
            }
            ManagementObjectSearcher ekran = new ManagementObjectSearcher("Select * From Win32_VideoController");
            foreach (ManagementObject Mobject in ekran.Get())
            {
                label4.Text = "EKRAN KARTINIZ: " + " " + Mobject["name"].ToString();
            }
            string osSerial = null;
            string osVersionInfo = null;
            string OSinfo = null;
            ManagementObjectSearcher osInfo = new ManagementObjectSearcher("Select * From Win32_OperatingSystem");
            foreach (ManagementObject osInfoObj in osInfo.Get())
            {
                osSerial = (string)osInfoObj["Caption"];
                osVersionInfo = (string)osInfoObj["Version"];
                OSinfo = osSerial;  
            }
            label5.Text = "İŞLETİM SİSTEMİNİZ: " + " " + OSinfo;
            RegistryKey Rkey1 = Registry.LocalMachine;
            Rkey1 = Rkey1.OpenSubKey("HARDWARE\\DESCRIPTION\\System\\CentralProcessor\\0");
            label6.Text = "İŞLEMCİNİZ: " + " " + (string)Rkey1.GetValue("ProcessorNameString").ToString();
            ManagementObjectSearcher searcher = new ManagementObjectSearcher("SELECT * FROM Win32_DiskDrive");
            foreach (ManagementObject disk in searcher.Get())
            {
                label7.Text = "HDD BİLGİNİZ: " + disk["Model"];
            }
            int ekran_x = Screen.GetBounds(new Point(0, 0)).Width;
            int ekran_y = Screen.GetBounds(new Point(0, 0)).Height;
            label9.Text = "EKRAN ÇÖZÜNÜRLÜĞÜNÜZ: " + Convert.ToString(ekran_x + "x" + ekran_y);
            String bilgisayarAdi = Dns.GetHostName();
            label10.Text = "BİLGİSAYAR ADINIZ: " + bilgisayarAdi;
            String ipAdresi = Dns.GetHostByName(bilgisayarAdi).AddressList[0].ToString();
            label11.Text = "IP ADRESİNİZ: " + ipAdresi;
            SelectQuery Sq = new SelectQuery("Win32_DesktopMonitor");
            ManagementObjectSearcher objOSDetails = new ManagementObjectSearcher(Sq);
            ManagementObjectCollection osDetailsCollection = objOSDetails.Get();
            StringBuilder sb = new StringBuilder();
            foreach (ManagementObject mo in osDetailsCollection)
            {
                sb.AppendLine(string.Format("MONİTÖRÜNÜZ: {0}", (string)mo["PNPDeviceID"]));
            }
            label12.Text = sb.ToString();
            ManagementObjectSearcher searcher1 =
                    new ManagementObjectSearcher("root\\CIMV2",
                    "SELECT * FROM Win32_ComputerSystem");
            StringBuilder ag = new StringBuilder();
            foreach (ManagementObject queryObj in searcher1.Get())
            {
                label13.Text = string.Format("PC MARKA: {0}", queryObj["Manufacturer"]) + " " + string.Format("MODEL: {0}", queryObj["Model"]);
            }
// Veri Tabanı Ekleme

using System;
using System.Data;
using System.Data.SqlClient;
using System.Text;
using System.Windows.Forms;
namespace EnvanterProgramı
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }       
        static string conString="Server=Envanter-;Database=EnvanterTakip;Uid=sa;Password=envanter;";
        //Bu veritabanına bağlanmak için gerekli olan bağlantı cümlemiz.Bu satıra dikkat edelim.
        //Sql Servera bağlanırken kullandığımız bilgileri ve veritabanı ismini yazıyoruz.
        SqlConnection baglanti = newSqlConnection(conString);
        //bağlantı cümlemizi kullanarak bir SqlConnection bağlantısı oluşturuyoruz.
        private void btnKaydet_Click(object sender, EventArgs e)
        {
            //Hataları engellemeye yönelik olarak, tüm veritabanı işlemlerini try-catch blokları arasında yapmaya özen gösterin.
            try
            {
                if(baglanti.State==ConnectionState.Closed)
                baglanti.Open();
                // Bağlantımızı kontrol ediyoruz, eğer kapalıysa açıyoruz.
                string kayit = "insert into musteriler(tcno,isim,soyisim,telefon,adres) values (@tcno,@isim,@soyisim,@telefon,@adres)";
                // müşteriler tablomuzun ilgili alanlarına kayıt ekleme işlemini gerçekleştirecek sorgumuz.
                SqlCommand komut = newSqlCommand(kayit, baglanti);
                //Sorgumuzu ve baglantimizi parametre olarak alan bir SqlCommand nesnesi oluşturuyoruz.
             komut.Parameters.AddWithValue("@", txtişletim sistemi.Text);
             komut.Parameters.AddWithValue("@", txtişlemci.Text);
             komut.Parameters.AddWithValue("@", txtram.Text);
             komut.Parameters.AddWithValue("@", txtekran kartı.Text);
             komut.Parameters.AddWithValue("@", txtekran çözünürlüğü.Text);
             komut.Parameters.AddWithValue("@", txthdd bilgisi.Text);
             komut.Parameters.AddWithValue("@", txtip adresi.Text);
             komut.Parameters.AddWithValue("@", txtmac adresi.Text);
             komut.Parameters.AddWithValue("@", txtbilgisayar adı.Text);
             komut.Parameters.AddWithValue("@", txtPc marka.Text);
             komut.Parameters.AddWithValue("@", txtmonitör.Text);
                //Parametrelerimize Form üzerinde ki kontrollerden girilen verileri aktarıyoruz.
                komut.ExecuteNonQuery();
                //Veritabanında değişiklik yapacak komut işlemi bu satırda gerçekleşiyor.
                baglanti.Close();
                MessageBox.Show("Bilgisayar Kayıt İşlemi Gerçekleşti.");
            }
            catch (Exception hata)
            {
                MessageBox.Show("İşlem Sırasında Hata Oluştu." + hata.Message);
            }
        }
    }
}
//Veri Tabanı Ekleme
        }          
        private void Form1_Load(object sender, EventArgs e)
        {
            label1.Visible = false;
            label14.Visible = false;
            label15.Visible = false;
            label16.Visible = false;
            this.Text = "İNÖNÜ ÜNİVERSİTESİ - BİLGİ İŞLEM DAİRE BAŞKANLIĞI";
            this.BackColor = Color.FromArgb(224, 238, 224);
            this.Font = SystemFonts.IconTitleFont;
            label8.Font = new Font(label8.Font, FontStyle.Bold);
            label1.Font = new System.Drawing.Font(label1.Font.Name, 12F, FontStyle.Bold);
            label8.Font = new System.Drawing.Font(label8.Font.Name, 7F, FontStyle.Bold);
            label2.Font = new Font(label2.Font, FontStyle.Bold);
            label3.Font = new Font(label3.Font, FontStyle.Bold);
            label4.Font = new Font(label4.Font, FontStyle.Bold);
            label5.Font = new Font(label5.Font, FontStyle.Bold);
            label6.Font = new Font(label6.Font, FontStyle.Bold);
            label7.Font = new Font(label7.Font, FontStyle.Bold);
            
            label9.Font = new Font(label9.Font, FontStyle.Bold);
            label10.Font = new Font(label10.Font, FontStyle.Bold);
            label11.Font = new Font(label11.Font, FontStyle.Bold);
            label12.Font = new Font(label12.Font, FontStyle.Bold);
            label13.Font = new Font(label13.Font, FontStyle.Bold);
            label17.Font = new Font(label17.Font, FontStyle.Bold);
            label18.Font = new Font(label18.Font, FontStyle.Bold);
            pictureBox1.ImageLocation = "https://www.pngplay.com/wp-content/uploads/7/Computer-Screen-Icon-Transparent-File.png";
      pictureBox3.ImageLocation = "https://i.pinimg.com/originals/86/db/89/86db899a009f586382adeb7de90643cb.png";
            pictureBox3.SizeMode = System.Windows.Forms.PictureBoxSizeMode.Zoom;
            pictureBox1.SizeMode = System.Windows.Forms.PictureBoxSizeMode.Zoom;
            this.DesktopLocation = new Point(0, 0);
            this.MinimumSize = new System.Drawing.Size(this.Width, this.Height);
        }
    }
}
