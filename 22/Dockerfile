# -- Tahap Build Utama --
# Menggunakan base image NodeJS 22 yang stabil
FROM        node:22

# Metadata tentang image
LABEL       author="Rapz" maintainer="rapzzcode@gmail.com"

# Instalasi dan Konfigurasi Sistem
# Menggabungkan update, instalasi paket, dan cleanup dalam satu layer untuk efisiensi
RUN         apt update \
            && apt -y install ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev ca-certificates dnsutils tzdata zip tar curl build-essential libtool iputils-ping \
            && apt clean \
            && rm -rf /var/lib/apt/lists/*

# Membuat user 'container' yang akan digunakan untuk menjalankan aplikasi (non-root)
# Ini adalah praktik keamanan standar
RUN         useradd -m -d /home/container container

# Instalasi Global NPM & PM2
# PM2 diinstal secara global. Setelah instalasi ini, PM2 akan ada di PATH sistem.
RUN         npm install -g npm@latest \
            && npm install -g pm2

# -- Konfigurasi Lingkungan Runtime --
# Mengatur user yang akan menjalankan proses utama
USER        container

# Mengatur variabel lingkungan untuk user dan home directory
ENV         USER=container \
            HOME=/home/container

# Mengatur direktori kerja utama
WORKDIR     /home/container

# Menyalin entrypoint script ke dalam container
# Pastikan Anda memiliki file 'entrypoint.sh' di direktori yang sama dengan Dockerfile ini
COPY        ./entrypoint.sh /entrypoint.sh

# Mengatur izin eksekusi untuk entrypoint script
RUN         chmod +x /entrypoint.sh

# Perintah default saat container dijalankan
# CMD akan menjalankan /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
