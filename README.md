# Qiskit ile Kuantum Dolanıklık (Bell Durumu) Simülasyonu

Bu proje, kuantum mekaniğinin en temel ve en şaşırtıcı fenomenlerinden biri olan **kuantum dolanıklığını** en basit örneği olan **Bell durumu** üzerinden adım adım simüle etmektedir.

---

## 🎯 Projenin Amacı
Albert Einstein ın *"ürkütücü uzaktan etki"* olarak adlandırdığı dolanıklık iki veya daha fazla parçacığın kaderini aralarındaki mesafe ne olursa olsun anında birbirine bağlar.  
Bu projenin amacı bu soyut ve karmaşık görünen konsepti IBM'in açık kaynaklı **Qiskit** kütüphanesi kullanarak somut ve anlaşılır bir deneye dönüştürmektir.

**Bu simülasyon ile cevap aranan sorular:**
- Birkaç satır Python koduyla bir kuantum devresi nasıl kurulur?
- Süperpozisyon ve dolanıklık gibi temel kuantum durumları pratikte nasıl yaratılır?
- Teorik olarak öngörülen sonuçlar simülasyon ortamında doğrulanabilir mi?

---

## 🛠️ Kullanılan Teknolojiler
- **Dil:** Python  
- **Kütüphaneler:** Qiskit, Qiskit Aer, Matplotlib  
- **Ortam:** Google Colab / Jupyter Notebook  

---

## 🔬 Simülasyon Adımları

Simülasyonumuz, **|Φ+⟩ Bell durumu**nu yaratmayı hedefler.  
Bu durumun kuralı basittir:  
> "İki kübitin ölçüm sonuçları her zaman aynı olmalıdır."  
Yani, sadece **00** veya **11** sonuçlarını görmeyi bekliyoruz.

---

## 💻 Kod

```python
# Gerekli kütüphaneleri kur
# !pip install qiskit[visualization] qiskit-aer

from qiskit import QuantumCircuit
from qiskit.visualization import plot_histogram
from qiskit_aer import AerSimulator
import matplotlib.pyplot as plt

# 1. Devreyi oluştur: 2 kuantum kübit ve 2 klasik bit
circuit = QuantumCircuit(2, 2)

# 2. Süperpozisyon yarat: 0. kübite Hadamard kapısı uygula
circuit.h(0)

# 3. Dolanıklık yarat: 0. kübiti kontrol, 1. kübiti hedef olarak CNOT kapısı uygula
circuit.cx(0, 1)

# 4. Ölçüm yap: Kübitleri klasik bitlere kaydet
circuit.measure([0, 1], [0, 1])

# 5. Simülatörü çalıştır: Devreyi 1024 kez tekrarla
simulator = AerSimulator()
job = simulator.run(circuit, shots=1024)
result = job.result()
counts = result.get_counts(circuit)

# Sonuçları yazdır ve histogramı çizdir
print("Ölçüm Sonuçları:", counts)
plot_histogram(counts, title='Bell Durumu Simülasyon Sonuçları')
plt.show()
