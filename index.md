[TOC]

#Kinect v1/OpenCV2.4.7/PCL1.7.2(x86)環境で開発を行う際の設定メモ 2015年11月20日
---------------------------------------------------------------------------------------------------------------------

##はじめに
Visual Studioを用いてKinect v1/OpenCV/PCLを利用するには，プロジェクト設定が必要です。
プロジェクトを作成するごとに確認しながら入力するのは大変なので，ここでテンプレートを作成しておきます。
このテンプレートをコピーしてVisual Studioのプロジェクトの設定に適用することで，すぐにこれらを利用することができます。

##開発環境に関して
今回の開発環境は以下のようになっています。
- Visual Studio 2013 Professional
- Kinect SDK v1.7
- OpenCV 2.4.7
- PCL 1.7.2

##環境変数に関して
Kinect v1/OpenCV/PCLを利用するためには，はじめに環境変数を設定しなければなりません。
ここでは，環境変数の設定方法を説明します。
1. コントロールパネルを開く
2. 「システムとセキュリティ」をクリック
3. 「システム」をクリック
4. 「システムの詳細設定」をクリック
5. 「環境変数」をクリック
6. 変数"Path"に以下を記入する。複数記入する場合は区切りにセミコロン';'を付ける必要があるので注意。
```
C:\OpenCV2.4.7\build\x86\vc11\bin;
C:\Program Files (x86)\PCL 1.7.2\bin;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\FLANN\bin;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\VTK\bin;
```

基本的には，利用する全てのライブラリのbinフォルダまでのパスを設定すれば良いです。
自分のインストールしたバージョンと異なる場合は，適宜読み替えてbinフォルダまでのパスを設定してください。

##Visual Studioのプロジェクトの設定ページヘの遷移手順
Kinect v1/OpenCV/PCLを利用するためにVisual Studioに設定を行っていくわけですが，ここでは設定ページヘの遷移手順を説明します。
遷移手順は以下のようになります。
1. Visual Studioを開く
2. 対象のプロジェクトを開く
3. 「プロジェクト」
4. 「(プロジェクト名)のプロパティ」
5. 「構成：」から「Debugモード」／「Releaseモード」を選択し，以下の設定を行ってください。

---------------------------------------------------------------------------------------------------------------------
##追加のインクルードファイルの設定
まずは，ヘッダファイルをインクルードするためのディレクトリを設定します。
環境変数と同じように複数のパスを設定する場合はセミコロン';'で区切ってください。
設定画面への遷移手順は以下のようになります。
1. 「構成プロパティ」
2. 「C/C++」
3. 「全般」
4. 「追加のインクルードディレクトリ」

###【Debugモード】
```
C:\Program Files\Microsoft SDKs\Kinect\v1.8\inc;
C:\OpenCV2.4.7\build\include\opencv2;
C:\OpenCV2.4.7\build\include;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\Eigen\eigen3;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\VTK\include\vtk-6.2;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\Boost\include\boost-1_57;
C:\Program Files (x86)\PCL 1.7.2\include\pcl-1.7;
```

###【Releaseモード】
```
C:\Program Files\Microsoft SDKs\Kinect\v1.8\inc;
C:\OpenCV2.4.7\build\include\opencv2;
C:\OpenCV2.4.7\build\include;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\Eigen\eigen3;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\VTK\include\vtk-6.2;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\Boost\include\boost-1_57;
C:\Program Files (x86)\PCL 1.7.2\include\pcl-1.7;
```

※"C:\OpenCV2.4.7\build\include\opencv2"と"C:\OpenCV2.4.7\build\include"に関する注意
本来は"C:\OpenCV2.4.7\build\include"のみで良かったのですが，PCLで"flann.hpp"を用いる際にパスが異なっていると，エラーが出てしまいました。
これを解決するために両方記述しています。
参考URL : http://13mzawa2.hateblo.jp/entry/2015/06/10/221750

---------------------------------------------------------------------------------------------------------------------

##追加のライブラリディレクトリの設定
次に，ライブラリをリンクするためのディレクトリを設定します。
こちらも複数のパスを設定する場合はセミコロン';'で区切ってください。
設定画面への遷移手順は以下のようになります。
1. 「構成プロパティ」
2. 「リンカー」
3. 「追加のライブラリディレクトリ」

###【Debugモード】
```
C:\Program Files\Microsoft SDKs\Kinect\v1.8\lib\x86;
C:\OpenCV2.4.7\build\x86\vc11\lib;
C:\Program Files (x86)\PCL 1.7.2\lib;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\VTK\lib;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\Boost\lib;
```

###【Releaseモード】
```
C:\Program Files\Microsoft SDKs\Kinect\v1.8\lib\x86;
C:\OpenCV2.4.7\build\x86\vc11\lib;
C:\Program Files (x86)\PCL 1.7.2\lib;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\VTK\lib;
C:\Program Files (x86)\PCL 1.7.2\3rdParty\Boost\lib;
```

---------------------------------------------------------------------------------------------------------------------

##ライブラリファイルの追加
追加のライブラリディレクトリを設定したあとは，そのライブラリ内にある".lib"ファイルを利用できるようにするための設定を行います。
ファイル数がかなり多くなりますが，今までと同様に規定の場所に記入するだけです。
こちらもファイル間の区切りにはセミコロン';'を使うようにしてください。
設定画面への遷移手順は以下のようになります。
1. 「構成プロパティ」
2. 「リンカー」
3. 「入力」

###【Debugモード】
```
Kinect10.lib;
opencv_video247d.lib;opencv_core247d.lib;opencv_imgproc247d.lib;opencv_highgui247d.lib;
pcl_common_debug.lib;pcl_features_debug.lib;pcl_filters_debug.lib;pcl_io_debug.lib;pcl_io_ply_debug.lib;pcl_kdtree_debug.lib;pcl_keypoints_debug.lib;pcl_octree_debug.lib;pcl_outofcore_debug.lib;pcl_people_debug.lib;pcl_recognition_debug.lib;pcl_registration_debug.lib;pcl_sample_consensus_debug.lib;pcl_search_debug.lib;pcl_segmentation_debug.lib;pcl_surface_debug.lib;pcl_tracking_debug.lib;pcl_visualization_debug.lib;
vtkalglib-6.2-gd.lib;vtkChartsCore-6.2-gd.lib;vtkCommonColor-6.2-gd.lib;vtkCommonComputationalGeometry-6.2-gd.lib;vtkCommonCore-6.2-gd.lib;vtkCommonDataModel-6.2-gd.lib;vtkCommonExecutionModel-6.2-gd.lib;vtkCommonMath-6.2-gd.lib;vtkCommonMisc-6.2-gd.lib;vtkCommonSystem-6.2-gd.lib;vtkCommonTransforms-6.2-gd.lib;vtkDICOMParser-6.2-gd.lib;vtkDomainsChemistry-6.2-gd.lib;vtkexoIIc-6.2-gd.lib;vtkexpat-6.2-gd.lib;vtkFiltersAMR-6.2-gd.lib;vtkFiltersCore-6.2-gd.lib;vtkFiltersExtraction-6.2-gd.lib;vtkFiltersFlowPaths-6.2-gd.lib;vtkFiltersGeneral-6.2-gd.lib;vtkFiltersGeneric-6.2-gd.lib;vtkFiltersGeometry-6.2-gd.lib;vtkFiltersHybrid-6.2-gd.lib;vtkFiltersHyperTree-6.2-gd.lib;vtkFiltersImaging-6.2-gd.lib;vtkFiltersModeling-6.2-gd.lib;vtkFiltersParallel-6.2-gd.lib;vtkFiltersParallelImaging-6.2-gd.lib;vtkFiltersProgrammable-6.2-gd.lib;vtkFiltersSelection-6.2-gd.lib;vtkFiltersSMP-6.2-gd.lib;vtkFiltersSources-6.2-gd.lib;vtkFiltersStatistics-6.2-gd.lib;vtkFiltersTexture-6.2-gd.lib;vtkFiltersVerdict-6.2-gd.lib;vtkfreetype-6.2-gd.lib;vtkftgl-6.2-gd.lib;vtkGeovisCore-6.2-gd.lib;vtkgl2ps-6.2-gd.lib;vtkhdf5-6.2-gd.lib;vtkhdf5_hl-6.2-gd.lib;vtkImagingColor-6.2-gd.lib;vtkImagingCore-6.2-gd.lib;vtkImagingFourier-6.2-gd.lib;vtkImagingGeneral-6.2-gd.lib;vtkImagingHybrid-6.2-gd.lib;vtkImagingMath-6.2-gd.lib;vtkImagingMorphological-6.2-gd.lib;vtkImagingSources-6.2-gd.lib;vtkImagingStatistics-6.2-gd.lib;vtkImagingStencil-6.2-gd.lib;vtkInfovisCore-6.2-gd.lib;vtkInfovisLayout-6.2-gd.lib;vtkInteractionImage-6.2-gd.lib;vtkInteractionStyle-6.2-gd.lib;vtkInteractionWidgets-6.2-gd.lib;vtkIOAMR-6.2-gd.lib;vtkIOCore-6.2-gd.lib;vtkIOEnSight-6.2-gd.lib;vtkIOExodus-6.2-gd.lib;vtkIOExport-6.2-gd.lib;vtkIOGeometry-6.2-gd.lib;vtkIOImage-6.2-gd.lib;vtkIOImport-6.2-gd.lib;vtkIOInfovis-6.2-gd.lib;vtkIOLegacy-6.2-gd.lib;vtkIOLSDyna-6.2-gd.lib;vtkIOMINC-6.2-gd.lib;vtkIOMovie-6.2-gd.lib;vtkIONetCDF-6.2-gd.lib;vtkIOParallel-6.2-gd.lib;vtkIOParallelXML-6.2-gd.lib;vtkIOPLY-6.2-gd.lib;vtkIOSQL-6.2-gd.lib;vtkIOVideo-6.2-gd.lib;vtkIOXML-6.2-gd.lib;vtkIOXMLParser-6.2-gd.lib;vtkjpeg-6.2-gd.lib;vtkjsoncpp-6.2-gd.lib;vtklibxml2-6.2-gd.lib;vtkmetaio-6.2-gd.lib;vtkNetCDF-6.2-gd.lib;vtkNetCDF_cxx-6.2-gd.lib;vtkoggtheora-6.2-gd.lib;vtkParallelCore-6.2-gd.lib;vtkpng-6.2-gd.lib;vtkproj4-6.2-gd.lib;vtkRenderingAnnotation-6.2-gd.lib;vtkRenderingContext2D-6.2-gd.lib;vtkRenderingContextOpenGL-6.2-gd.lib;vtkRenderingCore-6.2-gd.lib;vtkRenderingFreeType-6.2-gd.lib;vtkRenderingFreeTypeOpenGL-6.2-gd.lib;vtkRenderingGL2PS-6.2-gd.lib;vtkRenderingImage-6.2-gd.lib;vtkRenderingLabel-6.2-gd.lib;vtkRenderingLIC-6.2-gd.lib;vtkRenderingLOD-6.2-gd.lib;vtkRenderingOpenGL-6.2-gd.lib;vtkRenderingVolume-6.2-gd.lib;vtkRenderingVolumeOpenGL-6.2-gd.lib;vtksqlite-6.2-gd.lib;vtksys-6.2-gd.lib;vtktiff-6.2-gd.lib;vtkverdict-6.2-gd.lib;vtkViewsContext2D-6.2-gd.lib;vtkViewsCore-6.2-gd.lib;vtkViewsInfovis-6.2-gd.lib;vtkzlib-6.2-gd.lib;

```
###【Releaseモード】
```
Kinect10.lib;
opencv_video247.lib;opencv_core247.lib;opencv_imgproc247.lib;opencv_highgui247.lib;
pcl_common_release.lib;pcl_features_release.lib;pcl_filters_release.lib;pcl_io_release.lib;pcl_io_ply_release.lib;pcl_kdtree_release.lib;pcl_keypoints_release.lib;pcl_octree_release.lib;pcl_outofcore_release.lib;pcl_people_release.lib;pcl_recognition_release.lib;pcl_registration_release.lib;pcl_sample_consensus_release.lib;pcl_search_release.lib;pcl_segmentation_release.lib;pcl_surface_release.lib;pcl_tracking_release.lib;pcl_visualization_release.lib;
vtkalglib-6.2.lib;vtkChartsCore-6.2.lib;vtkCommonColor-6.2.lib;vtkCommonComputationalGeometry-6.2.lib;vtkCommonCore-6.2.lib;vtkCommonDataModel-6.2.lib;vtkCommonExecutionModel-6.2.lib;vtkCommonMath-6.2.lib;vtkCommonMisc-6.2.lib;vtkCommonSystem-6.2.lib;vtkCommonTransforms-6.2.lib;vtkDICOMParser-6.2.lib;vtkDomainsChemistry-6.2.lib;vtkexoIIc-6.2.lib;vtkexpat-6.2.lib;vtkFiltersAMR-6.2.lib;vtkFiltersCore-6.2.lib;vtkFiltersExtraction-6.2.lib;vtkFiltersFlowPaths-6.2.lib;vtkFiltersGeneral-6.2.lib;vtkFiltersGeneric-6.2.lib;vtkFiltersGeometry-6.2.lib;vtkFiltersHybrid-6.2.lib;vtkFiltersHyperTree-6.2.lib;vtkFiltersImaging-6.2.lib;vtkFiltersModeling-6.2.lib;vtkFiltersParallel-6.2.lib;vtkFiltersParallelImaging-6.2.lib;vtkFiltersProgrammable-6.2.lib;vtkFiltersSelection-6.2.lib;vtkFiltersSMP-6.2.lib;vtkFiltersSources-6.2.lib;vtkFiltersStatistics-6.2.lib;vtkFiltersTexture-6.2.lib;vtkFiltersVerdict-6.2.lib;vtkfreetype-6.2.lib;vtkftgl-6.2.lib;vtkGeovisCore-6.2.lib;vtkgl2ps-6.2.lib;vtkhdf5-6.2.lib;vtkImagingColor-6.2.lib;vtkImagingCore-6.2.lib;vtkImagingFourier-6.2.lib;vtkImagingGeneral-6.2.lib;vtkImagingHybrid-6.2.lib;vtkImagingMath-6.2.lib;vtkImagingMorphological-6.2.lib;vtkImagingSources-6.2.lib;vtkImagingStatistics-6.2.lib;vtkImagingStencil-6.2.lib;vtkInfovisCore-6.2.lib;vtkInfovisLayout-6.2.lib;vtkInteractionImage-6.2.lib;vtkInteractionStyle-6.2.lib;vtkInteractionWidgets-6.2.lib;vtkIOAMR-6.2.lib;vtkIOCore-6.2.lib;vtkIOEnSight-6.2.lib;vtkIOExodus-6.2.lib;vtkIOExport-6.2.lib;vtkIOGeometry-6.2.lib;vtkIOImage-6.2.lib;vtkIOImport-6.2.lib;vtkIOInfovis-6.2.lib;vtkIOLegacy-6.2.lib;vtkIOLSDyna-6.2.lib;vtkIOMINC-6.2.lib;vtkIOMovie-6.2.lib;vtkIONetCDF-6.2.lib;vtkIOParallel-6.2.lib;vtkIOParallelXML-6.2.lib;vtkIOPLY-6.2.lib;vtkIOSQL-6.2.lib;vtkIOVideo-6.2.lib;vtkIOXML-6.2.lib;vtkIOXMLParser-6.2.lib;vtkjpeg-6.2.lib;vtkjsoncpp-6.2.lib;vtklibxml2-6.2.lib;vtkmetaio-6.2.lib;vtkNetCDF-6.2.lib;vtkoggtheora-6.2.lib;vtkParallelCore-6.2.lib;vtkpng-6.2.lib;vtkproj4-6.2.lib;vtkRenderingAnnotation-6.2.lib;vtkRenderingContext2D-6.2.lib;vtkRenderingContextOpenGL-6.2.lib;vtkRenderingCore-6.2.lib;vtkRenderingFreeType-6.2.lib;vtkRenderingFreeTypeOpenGL-6.2.lib;vtkRenderingGL2PS-6.2.lib;vtkRenderingImage-6.2.lib;vtkRenderingLabel-6.2.lib;vtkRenderingLIC-6.2.lib;vtkRenderingLOD-6.2.lib;vtkRenderingOpenGL-6.2.lib;vtkRenderingVolume-6.2.lib;vtkRenderingVolumeOpenGL-6.2.lib;vtksqlite-6.2.lib;vtksys-6.2.lib;vtktiff-6.2.lib;vtkverdict-6.2.lib;vtkViewsContext2D-6.2.lib;vtkViewsCore-6.2.lib;vtkViewsInfovis-6.2.lib;vtkzlib-6.2.lib;
```

ファイル数が多く読みにくいですが，上から順にKinect, OpenCV, PCL, VTKのライブラリを使用するための設定です。
今後，バージョンを変更してこのリストを更新する際に，全てのライブラリファイルを手動で記入するのは大変な作業であるため，
フォルダ内のリストをクリップボードに保存してくれるフリーソフト"ファイル名一覧取得"(URL : http://www.vector.co.jp/soft/win95/util/se391324.html) などを利用すると便利です。

また、VTKはDebug用とRelease用のファイルが多数混在しており，各モードごとのリストを取得するのは困難であるため，Windowsの検索機能を利用して，それぞれのファイルを絞り込みリスト取得用のディレクトリにコピーしてからリストを取得すると良いでしょう。
この時，Windowsの検索機能では完全一致で検索してくれないため，以下のようにして検索すると絞り込めます。

[TOC]
`gd.lib //Debugモードを絞り込むとき`
`2 NOT gd //Releaseモードを絞り込むとき`

---------------------------------------------------------------------------------------------------------------------

これらの設定が完了したら，無事にKinect v1/OpenCV/PCLを利用するプログラムの開発が行えるはずです。