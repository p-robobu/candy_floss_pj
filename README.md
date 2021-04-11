# candy_floss_pj
# CandyFloss
Let's make candy floss with a baxter!!

Baxterのレポジトリが古いためBaxterのソースを一部変更したものをビルドする必要がある  
そのため、以前のセットアップを使用したあとにソースを置き換える必要がある  
また、Baxterラクチンセットアップ中にmoveitのエラーが出るため以下を参照にして追加のコードを打つ必要がある  
https://answers.ros.org/question/304475/cannot-build-moveit/  
以下にその手順を示す

## Baxterラクチンセットアップ
1. catkin_wsの下にリポジトリをクローンして`setup.sh`を実行する。  
   パスワード入力求められたり、いろいろ聞かれるけど適当に対応してあげて

       $ cd catkin_ws/src
       $ clone https://github.com/p-robobu/candy_floss_pkg_old.git
       $ bash ./setup.sh

## Baxterラクチンセットアップのエラー修正
1. 「moveit_visual_toolsConfig.cmake」に関してエラーが出るため以下のコマンドを実行する

       $ rosdep install --from-paths src --ignore-src -r -y

## Baxterの修正したソースの導入
1. 以前のソースはビルドは可能だが実行時にエラーが出るため、修正したソースに変更する  
まずは、現在のソースを削除する  

       $ コマンドでやりたかったのですが、rmコマンドが怖いので自分でポチポチしてください  

2. 修正後のソースを導入する  

       $ cd catkin_ws/src
       $ clone https://github.com/p-robobu/candy_floss_pj.git
       $ cd ..
       $ cd catkin_make
これで修正後のソースでビルドができるはずです

## baxter ノードから指定ポイントへ動かす

1. baxterのガゼボを起動

       $ roslaunch baxter_gazebo baxter_world.launch

2. baxterの腕を初期位置に移動させるノード ※ムーブイット系を実行中は動作できない
   最初に初期位置にしないと自作ノードのポイントに動作できないので必ず実行する

       $ rosrun baxter_tools tuck_arms.py -u

3. これをしないとムーブイットが動作しない

       $ rosrun baxter_interface joint_trajectory_action_server.py

4. baxterのムーブイットrvizを起動するコマンド ※これを起動しないと自作ノードが動かない

       $ roslaunch baxter_moveit_config baxter_grippers.launch

5. 自作ノードで腕を動かす

       $ rosrun candy_floss test_arm_moving 

## 指定ポイントの作り方

1. baxterのガゼボを起動

       $ roslaunch baxter_gazebo baxter_world.launch

2. キーボードでバクスターの腕を指定したいポイントへ動かす 起動したら一度エンターを押す
   ※rosrun baxter_interface joint_trajectory_action_server.py が動いているときは動作しない

       $ rosrun baxter_examples joint_position_keyboard.py

3. 現在のハンドの姿勢の座標を取得する 
   poseのpositionとorientationの値をメモしてtest_arm_moving.cppに記入する

       $ rostopic echo /robot/limb/left/endpoint_state 
       $ rostopic echo /robot/limb/right/endpoint_state 



# candy_floss_pj_old
# CandyFloss
Let's make candy floss with a baxter!!

## Baxterラクチンセットアップ
1. catkin_wsの下にリポジトリをクローンして`setup.sh`を実行する。
   パスワード入力求められたり、いろいろ聞かれるけど適当に対応してあげて

       $ cd catkin_ws/src
       $ clone https://github.com/p-robobu/candy_floss.git
       $ bash ./setup.sh

## baxter ノードから指定ポイントへ動かす

1. baxterのガゼボを起動

       $ roslaunch baxter_gazebo baxter_world.launch

2. baxterの腕を初期位置に移動させるノード ※ムーブイット系を実行中は動作できない
   最初に初期位置にしないと自作ノードのポイントに動作できないので必ず実行する

       $ rosrun baxter_tools tuck_arms.py -u

3. これをしないとムーブイットが動作しない

       $ rosrun baxter_interface joint_trajectory_action_server.py

4. baxterのムーブイットrvizを起動するコマンド ※これを起動しないと自作ノードが動かない

       $ roslaunch baxter_moveit_config baxter_grippers.launch

5. 自作ノードで腕を動かす

       $ rosrun candy_floss test_arm_moving 

## 指定ポイントの作り方

1. baxterのガゼボを起動

       $ roslaunch baxter_gazebo baxter_world.launch

2. キーボードでバクスターの腕を指定したいポイントへ動かす 起動したら一度エンターを押す
   ※rosrun baxter_interface joint_trajectory_action_server.py が動いているときは動作しない

       $ rosrun baxter_examples joint_position_keyboard.py

3. 現在のハンドの姿勢の座標を取得する 
   poseのpositionとorientationの値をメモしてtest_arm_moving.cppに記入する

       $ rostopic echo /robot/limb/left/endpoint_state 
       $ rostopic echo /robot/limb/right/endpoint_state 


