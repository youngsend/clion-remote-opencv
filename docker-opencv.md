- clionのremote modeでやるとき、いつも`cv::nameWindow()`でSIGABRTエラーが出ている。多分GUIを使えていない。

- 直接にdocker container内で実行しても同じエラー：![](img/cvNamedWindow-debug-2020-08-16-00-24-46.png)

- `libgtk2.0-dev`や`pkg-config`をインストールしよう！

- インストールしたが、`Gtk-WARNING: cannot open display`のエラー：![](img/gtk-warning-cannot-open-display-2020-08-16-00-58-12.png)

- `xhost +`でできたが、clionでは相変わらずできない。今後解決しよう。![](img/docker-gui-2020-08-16-12-04-01.png)

- 下記の３ステップで実行している。

  ```bash
  docker run -d --cap-add sys_ptrace -p 2222:22 --name clion_remote_env -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix clion/remote-cpp-env:0.5
  
  xhost +
  
  docker exec -it clion_remote_env /bin/bash
  
  # cmake, make, 実行, exit
  
  xhost -
  ```

  - コードのマッピングはclionで設定しているので、containerをrunするときは指定していない（下記のマッピングはdocker run時指定してみたが、clionのdata transferと衝突したかもしれない、コードは全部消された）：![](img/source-code-mapping-2020-08-16-12-22-24.png)

- なので、今のやり方は、clionがeditor + file transfererを担当している。exec bashはmake (clionでもOK) + 実行を担当している。