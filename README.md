import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

class SalesAnalyzer:
    """
    Kelas untuk melakukan pembersihan dan analisis data penjualan.
    """
    def __init__(self, file_path):
        self.data = pd.read_csv(file_path)
        
    def clean_data(self):
        """
        Membersihkan data dengan menghapus missing values 
        dan mengonversi tipe data kolom tanggal.
        """
        # Menghapus duplikat
        self.data.drop_duplicates(inplace=True)
        # Menangani nilai kosong
        self.data.dropna(subset=['sales', 'date'], inplace=True)
        # Konversi ke datetime
        self.data['date'] = pd.to_datetime(self.data['date'])
        return "Data berhasil dibersihkan."

    def perform_eda(self):
        """
        Melakukan Exploratory Data Analysis (EDA) sederhana.
        """
        summary = self.data.describe()
        correlation = self.data.corr(numeric_only=True)
        return summary, correlation

    def visualize_trend(self):
        """
        Menampilkan tren penjualan bulanan menggunakan seaborn.
        """
        self.data['month'] = self.data['date'].dt.to_period('M')
        monthly_sales = self.data.groupby('month')['sales'].sum()
        
        plt.figure(figsize=(10, 6))
        sns.lineplot(x=monthly_sales.index.astype(str), y=monthly_sales.values)
        plt.title('Tren Penjualan Bulanan')
        plt.xticks(rotation=45)
        plt.show()

# --- Implementasi ---
if __name__ == "__main__":
    # Inisialisasi Analisis
    # Pastikan Anda memiliki file 'sales_data.csv'
    try:
        analyzer = SalesAnalyzer('sales_data.csv')
        print(analyzer.clean_data())
        
        stats, corr = analyzer.perform_eda()
        print("Ringkasan Statistik:\n", stats)
        
        analyzer.visualize_trend()
    except FileNotFoundError:
        print("File tidak ditemukan. Pastikan path file benar.")
