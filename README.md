# Kubernetes Orchestration & MLOps Deployment Guide

Bu depo; konteynerleştirilmiş uygulamaların orkestrasyonu, Kubernetes (K8s) altyapı yönetimi ve makine öğrenmesi (ML) modellerinin üretim ortamına (production) güvenli, ölçeklenebilir ve yüksek erişilebilir şekilde taşınması (MLOps) süreçlerine ait teknik rehberleri ve pratik uygulama dökümanlarını barındırmaktadır.

Yerel geliştirme ortamlarından (Minikube) başlayarak, Kubernetes mimarisinin çekirdek bileşenlerine (Pod, Deployment, Storage, Ingress) ve en nihayetinde akıllı servislerin (Flask ML API) orkestrasyonuna uzanan uçtan uca bir mühendislik kılavuzudur.

---

## 🚀 Depo İçeriği ve Yol Haritası

Dökümantasyonlar, temel altyapı araçlarından başlayıp ileri seviye MLOps mimarilerine doğru hiyerarşik bir sıra takip etmektedir:

### 🗺️ Yerel Geliştirme ve CLI Referansları
* **[Minikube Cheat Sheet](minikube-cheat-sheet.md):** Lokal Kubernetes kümesinin (cluster) ayağa kaldırılması, kaynak yönetimi, tünelleme ve sanal ortam optimizasyonları.
* **[2. Kubectl Cheat Sheet](2-kubectl-cheat-sheet.md):** Küme yönetiminde en sık kullanılan `kubectl` komutları, hızlı durum sorgulama, log analizi ve hata ayıklama (troubleshooting) referans kılavuzu.

### 🏗️ Kubernetes Çekirdek Bileşenleri Yönetimi
* **[3. Kubernetes Pod Management](3-kubernetes-pod-management.md):** Kubernetes'in en küçük yapı taşı olan Pod'ların yaşam döngüsü, deklaratif konfigürasyonları ve operasyonel yönetimi.
* **[4. Kubernetes Deployment Guide](4-kubernetes-deployment-guide.md):** Uygulamaların kesintisiz güncellenmesi (Rolling Update), replika yönetimi, ölçeklenme (scaling) stratejileri ve geri alma (rollback) mekanizmaları.

### 💾 Ağ ve Kalıcı Depolama Mimarisi
* **[5. Kubernetes Persistent Storage](5-kubernetes-persistent-storage.md):** Durumsal (stateful) uygulamalar için kalıcı veri yönetimi, `PersistentVolume (PV)`, `PersistentVolumeClaim (PVC)` ve StorageClass konseptleri.
* **[6. Kubernetes Ingress Routing](6-kubernetes-ingress-routing.md):** Küme dışındaki trafiğin servis katmanlarına güvenli ve yönlendirmeli şekilde aktarılması, HTTP/HTTPS Ingress kuralları ve reverse proxy mantığı.

### 🧠 MLOps ve Makine Öğrenmesi Dağıtım (Deployment) Pratikleri
* **[7. ML Model Deployment on Kubernetes](7-ml-model-deployment-kubernetes.md):** Eğitilmiş makine öğrenmesi modellerinin mikroservis mimarisine dönüştürülerek Kubernetes üzerinde prod seviyesinde ölçeklenmesi ilkeleri.
* **[8. Kubernetes Ingress with Flask ML API](8-kubernetes-ingress-flask-ml.md):** Flask ile yazılmış bir makine öğrenmesi API'sinin konteynerize edilmesi, K8s üzerinde ayağa kaldırılması ve Ingress yönlendirmesi ile dış dünyaya güvenli bir şekilde sunulması (Uçtan uca canlı senaryo).

---

## 🛠️ Öne Çıkan Mimari Yaklaşımlar

* **Modüler Yapı:** Altyapı ve yazılım katmanları tamamen birbirinden ayrılarak dekuple (decoupled) bir mimari gözetilmiştir.
* **Kalıcı Veri Güvencesi:** Modellerin ağırlık dosyaları (weights) ve uygulama logları için efemer (geçici) yapılar yerine kalıcı depolama (`Persistent Storage`) pratikleri uygulanmıştır.
* **Trafik Yönetimi:** Servislere doğrudan NodePort ile erişmek yerine, modern prod standartları gereği tek bir giriş noktası üzerinden **Ingress Controller** mimarisi konumlandırılmıştır.

---

## 📋 Gereksinimler & Başlangıç

Bu depodaki senaryoları yerel ortamınızda test etmek için aşağıdaki araçların kurulu olması önerilir:
* [Docker](https://www.docker.com/) (Konteyner motoru)
* [Minikube](https://minikube.sigs.k8s.io/docs/) (Yerel Kubernetes kümesi)
* [Kubectl](https://kubernetes.io/docs/tasks/tools/) (Kubernetes CLI aracı)

### Hızlı Başlatma
1. Yerel Kubernetes kümesini başlatın:
   ```bash
   minikube start --driver=docker
