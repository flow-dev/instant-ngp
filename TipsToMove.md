# 動画にCOLMAPを掛ける

```bash
python3 /home/z/work/instant-ngp/scripts/colmap2nerf.py --video_in Test_TM.mp4 --video_fps 10 --colmap_matcher sequential --run_colmap
```

* instant-ngp/data/hogeの下で実行することで,成果物のtransforms.jsonと画像pathが一致する.
* --colmap_matcher sequential が動画用の設定.
* --video_fpsで総枚数が100前後になるように調整する.
* COLMAP3.3以上の場合は,colmap2nerf.pyで書き変更が必要.--export_pathに引数名を変更.

```python
do_system(f"colmap mapper --database_path {db} --image_path {images} --export_path {sparse}")
```

# 静止画にCOLMAPを掛ける

```bash
python3 /home/z/work/instant-ngp/scripts/colmap2nerf.py --colmap_matcher exhaustive --run_colmap --images /home/z/work/instant-ngp/data/hoge/images
```

* instant-ngp/data/hogeの下で実行することで,成果物のtransforms.jsonと画像pathが一致する.
* --colmap_matcher exhaustive が静止画用の設定.

# InstantNeRFのGUI実行

```bash
./build/testbed --scene data/yamamoto/
```

# NeRFの結果をカメラワーク付き動画で書き出す

```bash
python3 scripts/run.py --mode nerf --scene data/nerf/fox --load_snapshot data/nerf/fox/base.msgpack --video_camera_path data/nerf/fox/base_cam.json --video_n_seconds 10 --video_fps 24 --width 1920 --height 1080
```

* base.msgpackは, train結果のsnapshot. InstantNeRFのGUIから保存できる.
* base_cam.jsonは, 指定したカメラワークのキーフレーム. InstantNeRFのGUIから保存できる.(撮影した画像位置を中心にずらすと良い)

# NeRFの結果をmeshで書き出す

```bash
python3 scripts/run.py --scene data/nerf/fox --load_snapshot data/nerf/fox/base.msgpack --save_mesh fox.obj
```

* obj or plyで書き出せるが、テクスチャは書き出せず.またmeshも荒い.
* 現時点ではリトポ用の域を出ない.将来に期待.


# Cmake 3.22.6以下でのコンフィグが必須

```bash
snap remove cmake
snap info cmake
sudo snap install cmake --channel=3.22/stable --classic
export CUDACXX=/usr/local/cuda-11.6/bin/nvcc
/snap/bin/cmake . -B build
```

* sudo snap install cmake --channel=3.22/stable --classic でcmakeのバージョン指定
* export CUDACXX=/usr/local/cuda-11.6/bin/nvccで,cuda指定
* snapでinstallしたcmakeは、/snap/bin/cmakeで呼べる

# COLOMAP3.3のインストール

```bash
sudo apt-get install colmap
```

* aptでCOLOMAP3.3が入る(Ubuntu18.0.4)
* gitからビルドすれば最新がインストールできる(多分動く)

# Ubuntu18.04にpython3.7を入れる

https://qiita.com/rurou/items/edbb06538e81e01ec387

* python3.6とpython3.7が入っていて, python3でpython3.7が呼ばれる.　/usr/bin/python3.6でpython3.6が呼ばれる.

https://ossyaritoori.hatenablog.com/entry/2020/09/17/Python2%E3%81%A83%E3%81%8C%E5%85%B1%E5%AD%98%E3%81%97%E3%81%A6%E3%81%84%E3%82%8B%E3%81%A8%E3%81%8D%E3%81%ABpip%E3%81%AB%E7%89%B9%E5%AE%9A%E3%81%AE%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8

* pip3は,python3.6を指しているので,以下のようにpython -m pipでターゲットを指定してインストールする.

```bash
/usr/bin/python3.6 -m pip install xx　#python3.6
/usr/local/bin/python3 -m pip install xx　#python3.7
```

# OptiX 7.3のインストール

https://developer.nvidia.com/designworks/optix/download

* NVIDIAの公式サイトからダウンロードする(ログイン必要)
* /NVIDIA-OptiX-SDK-7.5.0-linux64-x86_64/SDK/INSTALL-LINUX.txtにインストール方法が書いてある.
* /usr/loaclにインストールするようにcmakeをコンフィグする.optix.h等のインクルードファイル一式も/usr/localの同じ場所に置く

# Vulkan SDKのインストール

https://vulkan.lunarg.com/doc/view/latest/linux/getting_started_ubuntu.html

* 上記サイトのUbuntu18.04用の通りに行う

```bash
# Ubuntu 18.04 (Bionic Beaver)

wget -qO - http://packages.lunarg.com/lunarg-signing-key-pub.asc | sudo apt-key add -
sudo wget -qO /etc/apt/sources.list.d/lunarg-vulkan-bionic.list http://packages.lunarg.com/vulkan/lunarg-vulkan-bionic.list
sudo apt update
sudo apt install vulkan-sdk
```

# instant-ngpの最新を取り込む場合

```bash
git fetch
git pull
# https://github.com/NVlabs/instant-ngp/issues/415
git submodule sync --recursive
git submodule update --recursive
```
* git submoduleを忘れずに行う
