FROM ubuntu:20.04

ENV ANDROID_HOME="/opt/android-sdk" \
    PATH="/opt/android-sdk/tools/bin:/opt/flutter/bin:/opt/flutter/bin/cache/dart-sdk/bin:$PATH"

# Dockerコンテナ内のUbuntu OSに必要なパッケージをインストール
# commnd > /dev/null とすると標準出力を捨てることになる
# --no-install-recommendsを指定することでrecommnedsしているだけの必須でないパッケージはインストールされないようにする
RUN apt-get update > /dev/null \
    && apt-get -y install --no-install-recommends curl git lib32stdc++6 openjdk-8-jdk-headless unzip clang xserver-xorg pkg-config cmake ninja-build \
    && apt-get --purge autoremove > /dev/null \
    && apt-get autoclean > /dev/null \
    && rm -rf /var/lib/apt/lists/*

# Flutter SDKのインストール
RUN git clone https://github.com/flutter/flutter.git /opt/flutter

# Android SDKのインストール
RUN curl -s -O https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip \
    && mkdir /opt/android-sdk \
    && unzip sdk-tools-linux-4333796.zip -d /opt/android-sdk > /dev/null \
    && rm sdk-tools-linux-4333796.zip

RUN mkdir ~/.android \
    && echo 'count=0' > ~/.android/repositories.cfg \
    && yes | sdkmanager --licenses > /dev/null \
    && sdkmanager "tools" "build-tools;29.0.0" "platforms;android-29" "platform-tools" > /dev/null \
    && yes | sdkmanager --licenses > /dev/null \
    && flutter doctor -v \
    && chown -R root:root /opt

RUN ["apt-get", "update"]

RUN ["apt-get", "install", "-y", "vim"]

RUN ["apt-get", "install", "psmisc"]

# ワーキングディレクトリを設定
WORKDIR /home/flutter-tutorial

# Flutter linuxが使用できるようにする
RUN flutter config --enable-linux-desktop

# Flutter Webが使用できるようにする
RUN flutter config --enable-web

# Flutterの診断
RUN flutter doctor

# /dev/nullの末尾10行を標準出力に表示する．
# -fを指定することで，ファイルの内容が増えた場合，新しい末尾を追加表示する．
# RUNがビルド時にコンテナ内で実行されるのに対し，CMDは完成したイメージからコンテナを作成するときに実行される
CMD tail -f /dev/null
