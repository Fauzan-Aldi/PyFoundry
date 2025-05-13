# 🧪 PyFoundry – Pythonic Toolkit untuk Web3 dan CTF Ala Foundry

**PyFoundry** adalah sebuah implementasi Python dari toolkit populer [**Foundry**](https://github.com/foundry-rs/foundry), yang dirancang untuk memungkinkan pengguna berinteraksi dengan **Ethereum smart contracts** tanpa harus menginstal Foundry secara langsung.

Dengan gaya penggunaan yang menyerupai Foundry dan dukungan terhadap pustaka Python `web3.py`, PyFoundry menjadi solusi praktis untuk pengembang, peneliti keamanan blockchain, maupun peserta **CTF (Capture The Flag)** berbasis blockchain. PyFoundry juga menyertakan berbagai fitur tambahan yang secara khusus disesuaikan untuk menangani skenario tantangan di dunia CTF.

---

## 🚀 Fitur Utama PyFoundry

* 🔧 **Replikasi Fungsi Inti Foundry:**
  PyFoundry menghadirkan fitur-fitur utama seperti interaksi kontrak, deployment, dan pengecekan status, mirip dengan gaya CLI Foundry namun seluruhnya ditulis dalam Python.

* 🧑‍💻 **Tanpa Instalasi Foundry:**
  Tidak perlu setup lingkungan Rust atau menginstal Foundry secara manual—cukup gunakan Python dan instal `foundpy` via `pip`.

* 📡 **Integrasi dengan RPC dan Web3:**
  Menggunakan `web3.py`, PyFoundry memungkinkan komunikasi langsung dengan jaringan Ethereum melalui RPC URL serta mendukung penggunaan private key untuk transaksi.

* 🧠 **Mode CTF-Friendly:**
  PyFoundry menyediakan konfigurasi otomatis untuk tantangan dari platform seperti:

  * [Paradigm CTF](https://github.com/TCP1P/Paradigmctf-BlockChain-Infra-Extended)
  * [HackTheBox Blockchain](https://app.hackthebox.com/)

* ⚡ **Penggunaan Cepat dan Ringan:**
  Ideal digunakan pada Jupyter Notebook atau skrip Python biasa. Cukup beberapa baris kode untuk setup, mengirim transaksi, memanggil fungsi, dan mendapatkan flag.

---

## 📦 Instalasi

Instal PyFoundry cukup mudah menggunakan `pip`:

```sh
pip install PyFoundry
```

---

## 🛠️ Cara Penggunaan

### 1. 🔧 Inisialisasi Konfigurasi

```python
from PyFoundry import *

config.setup(
    rpc_url="http://rpc.url/",
    privkey="0xdeadbeef"
)
```

Jika kamu ingin menggunakan versi Solidity yang berbeda:

```python
config.change_solc_version("0.8.13")
```

Untuk pengguna tantangan CTF, bisa menggunakan helper khusus:

* **Paradigm CTF:**

  ```python
  result = config.from_tcp1p("http://103.178.153.113:30001")
  setup = Contract(result["setup_contract"], "Setup.Sol")
  ```

* **HackTheBox:**

  ```python
  result = config.from_htb(address="http://94.237.59.180:51929")
  setup = Contract(result["setupAddress"], "Setup.Sol")
  ```

Setelah tantangan terselesaikan, kamu bisa mengambil flag dengan:

```python
assert setup.call("isSolved")
print(config.flag())
```

---

### 2. 📞 Interaksi dengan Kontrak

Kamu bisa berinteraksi langsung menggunakan `cast` atau membuat objek `Contract`:

```python
setup_addr = "0xE162F3696944255cc2850eb73418D34023884D1E"
cast.send(setup_addr, "solve(bytes)", b"args123", value=ether(0.5))

# atau
setup = Contract(setup_addr, "Setup.Sol")
setup.send("solve", b"args123", value=ether(0.5))
```

---

### 3. 🚀 Deploy Kontrak

Deploy kontrak baru menggunakan `forge.create` atau `deploy_contract`:

```python
# Hanya alamat
attack = forge.create("Attack.sol:Attack", setup_addr, value=ether(1))

# Atau sebagai objek Contract langsung
attack = deploy_contract("Attack.sol", setup.address, value=ether(1))
