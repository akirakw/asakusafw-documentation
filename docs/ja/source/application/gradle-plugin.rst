====================================
Asakusa Gradle Plugin ユーザーガイド
====================================

この文書では、\ `Gradle <http://www.gradle.org/>`_\ にAsakusa Framework を使ったアプリケーションの開発やデプロイを行うための機能を追加するAsakusa Gradle Pluginについて説明します。

概要
====

Asakusa Gradle Pluginは、Asakusa Framework用のGradle拡張プラグイン群です。
このプラグインを利用することで、Gradleを利用してAsakusa Framework を使ったアプリケーションの開発やデプロイを行うことができます。

利用環境
--------

Asakusa Gradle Pluginを利用するにはJava(JDK)がインストールされている必要があります。
これらの導入方法については、 :doc:`../introduction/start-guide` - :ref:`startguide-development-environment` などを参考にしてください。

なお、Gradleのインストールについては、本書では `Gradleラッパー <http://www.gradle.org/docs/current/userguide/gradle_wrapper.html>`_ と呼ばれるGradleを利用するためのコマンドを使う方法を推奨しています。
この方法に沿ってGradleを利用する場合は前もってGradleをインストールする必要はありません。
詳しくは後述の `Asakusa Gradle Pluginの導入`_ を参考にしてください。

Gradleについて
--------------

`Gradle <http://www.gradle.org/>`_  はオープンソースプロジェクトとして開発されている様々なプラットフォームに対応したビルドシステムです。
シンプルかつ拡張性の高いビルド定義を行うためのDSLやプラグイン機構を持ち、他のビルドシステムとの連携を含む様々な方式に対応した依存性管理のメカニズムを有しているなど多くの特徴を持っています。

Gradleに関する詳しい情報は、以下のドキュメントなどを参考にしてください。

*  `Gradle Documentation <http://www.gradle.org/documentation>`_  (Gradleの公式ドキュメントサイト)
*  `Gradle 日本語ドキュメント <http://gradle.monochromeroad.com/docs/>`_  (公式ドキュメントの翻訳サイト)

関連プロダクト
==============

Shafu
-----

Shafu (車夫) は、 Asakusa Framework のバッチアプリケーション開発をサポートするEclipseプラグインです。

* :jinrikisha:`Shafu - Asakusa Gradle Plug-in Helper for Eclipse - <shafu.html>`

Shafuはバッチアプリケーション開発に Asakusa Gradle Plugin を利用する際に、Eclipseから透過的にビルドツール上の操作を行えます。
Shafuを使うことで、コマンドライン上でのビルドツールの操作が不要となり、Eclipse上でアプリケーション開発に必要なほとんどの作業を行うことができるようになります。

本書ではGradleを使った操作はコマンドライン上での利用手順として説明していますが、Eclipseを使った開発を行う場合は Shafu を利用してEclipse上からGradleの操作を行うことも可能です。

Asakusa Gradle Pluginの導入
===========================

Asakusa Gradle Plugin を利用する方法として、以下のいずれかの方法があります。

#. Asakusa Gradle Plugin 用プロジェクトテンプレートを使用する
#. ビルドスクリプトに個別にプラグイン利用に必要な設定を定義する

1)は、Asakusa Gradle Pluginの利用設定が行われたビルドスクリプト、及び標準的なプロジェクトレイアウトを含むプロジェクトテンプレートを利用する方法です。
Asakusa Gradle Pluginを使った標準的なアプリケーション開発環境を導入するにはこのテンプレートを使うと便利です。

このプロジェクトテンプレートには  `Gradleラッパー <http://www.gradle.org/docs/current/userguide/gradle_wrapper.html>`_  と呼ばれるGradleを利用するコマンドが含まれます。
このコマンドを利用することで、Gradle自体の導入設定は不要となり、すぐにこのプロジェクト上で開発を始めることができます。

現在のところ、プロジェクトテンプレートは利用するプラットフォームに応じた初期設定が導入された複数種類のテンプレートを公開しています。

2)は、テンプレートを使用せずフルスクラッチでビルドスクリプトの定義やプロジェクトレイアウトを作成する方法です。
この方法はGradleやAsakusa Gradle Pluginの利用に精通している必要がありますが、既存プロジェクトのマイグレーションなどで個別に設定を行う必要がある場合などでは、こちらの方法を検討してください。

以下では、1)のAsakusa Gradle Plugin 用プロジェクトテンプレートを利用した導入方法を解説します。
2)については、 :doc:`gradle-plugin-reference`  を参照してください。

.. _download-gradle-project-template:

プロジェクトテンプレートのダウンロード
--------------------------------------

プロジェクトテンプレートは、以下リンクからダウンロードします。

..  list-table:: プロジェクトテンプレートのダウンロード
    :widths: 3 4
    :header-rows: 1

    * - プロジェクトテンプレート
      - 説明
    * - `asakusa-spark-template-0.10.3.tar.gz <http://www.asakusafw.com/download/gradle-plugin/asakusa-spark-template-0.10.3.tar.gz>`_
      - :doc:`../spark/index` を利用するプロジェクトテンプレート
    * - `asakusa-m3bp-template-0.10.3.tar.gz <http://www.asakusafw.com/download/gradle-plugin/asakusa-m3bp-template-0.10.3.tar.gz>`_
      - :doc:`../m3bp/index` を利用するプロジェクトテンプレート
    * - `asakusa-mapreduce-template-0.10.3.tar.gz <http://www.asakusafw.com/download/gradle-plugin/asakusa-mapreduce-template-0.10.3.tar.gz>`_
      - :doc:`../mapreduce/index` を利用するプロジェクトテンプレート

また、Asakusa Frameworkの `サンプルプログラム集 (GitHub)`_ では、サンプルアプリケーションのソースコード一式を含むサンプルアプリケーションプロジェクトを公開しています。

..  _`サンプルプログラム集 (GitHub)`: http://github.com/asakusafw/asakusafw-examples

プロジェクトの配置
------------------

ダウンロードしたプロジェクトテンプレートのアーカイブを展開すると、プロジェクトテンプレート名をディレクトリ名に持つプロジェクトファイル一式が作成されます。
このディレクトリを開発するアプリケーションを示すプロジェクト名に変更して、作業用ディレクトリに配置してください。
サンプルアプリケーションを利用する場合も同様です。

以降本書では、ビルドの流れを解説するために `サンプルプログラム集 (GitHub)`_ に公開されているサンプルアプリケーションプロジェクト ``example-basic-spark`` を使って説明します。
このサンプルアプリケーションは、Asakusa on Sparkのプロジェクトテンプレートをベースにして作成されています。

ここでは、ダウンロードしたサンプルアプリケーションプロジェクト ``example-basic-spark`` を :file:`$HOME/workspace` に配置したものとします。

..  code-block:: sh

    cd ~/Downloads
    curl -OL https://github.com/asakusafw/asakusafw-examples/archive/0.10.3.tar.gz
    tar xf 0.10.3.tar.gz
    cp -a asakusafw-examples-0.10.3/example-basic-spark ~/workspace

プロジェクトレイアウト
----------------------

プロジェクトテンプレートを使ったアプリケーションプロジェクトのディレクトリ構成とテンプレートに含まれるファイルについて説明します。

..  tip::
    以降に示すプロジェクトレイアウトのディレクトリパスやファイル名は、Gradleの各プラグインやAsakusa Gradle Pluginの設定により変更可能です。詳しくはGradleのドキュメントや :doc:`gradle-plugin-reference` を参照してください。

プロジェクトルートディレクトリ
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

プロジェクトテンプレートから作成したプロジェクトディレクトリの直下には、以下のディレクトリ/ファイルが含まれます。

..  list-table:: プロジェクトレイアウト - プロジェクトルートディレクトリ
    :widths: 3 7
    :header-rows: 1

    * - ファイル/ディレクトリ
      - 説明
    * - :file:`build.gradle`
      - Gradleビルドスクリプト
    * - :file:`src`
      - プロジェクトのソースディレクトリ
    * - :file:`build`
      - プロジェクトのビルドディレクトリ（ビルド時に生成）
    * - :file:`gradlew`
      - Gradleラッパーコマンド (Unix)
    * - :file:`gradlew.bat`
      - Gradleラッパーコマンド (Windows)
    * - :file:`.buildtools`
      - Gradleラッパーライブラリ (Gradle Version: 4.7)

アプリケーション開発者は :file:`src` ディレクトリ配下を編集することでアプリケーションを開発します。
:file:`build` ディレクトリは :file:`src` ディレクトリ配下のファイルをビルドすることで生成される成果物が配置されます。

:file:`build` ディレクトリ配下のファイルはビルドの度に初期化、再作成されるため :file:`build` ディレクトリ配下のファイルは直接編集しないようにしてください。

GradleラッパーはGradleを使ったビルドを実行するために使用します。
Gradleラッパーに関するディレクトリ及びファイルは、Gradleラッパー自体のマイグレーションを行う場合を除き編集しないようにしてください。

..  attention::
    Gradleラッパーを使用せず、開発環境に対して個別にインストールしたGradleを使用することも出来ますが、この場合Asakusa Frameworkで未検証のバージョンのGradleを使用した場合に問題が発生する可能性があることに注意してください。
    本書ではGradleラッパーを使ってGradleに関する操作を説明しています。

ビルドスクリプト
~~~~~~~~~~~~~~~~

ビルドスクリプト( ``build.gradle`` )はプロジェクトのビルド設定を記述したGradle用のビルドスクリプトで、プロジェクトテンプレートに含まれるビルドスクリプトにはAsakusa Gradle Pluginを利用するための設定が記述されています。

..  literalinclude:: gradle-attachment/template-build.gradle
    :language: groovy
    :caption: build.gradle
    :name: build.gradle-gradle-plugin-1

プロジェクトテンプレートに含まれるビルドスクリプトには、以下の機能を利用するための設定があらかじめ定義されています。

* :doc:`Asakusa DSL <../dsl/index>`
* :doc:`Asakusa Data Model (DMDL) <../dmdl/index>`
* :doc:`テストドライバ <../testing/index>`
* :doc:`Direct I/O <../directio/index>`
* :doc:`WindGate <../windgate/index>`

ソースディレクトリ
~~~~~~~~~~~~~~~~~~

プロジェクトのソースディレクトリは大きくアプリケーション本体のコードを配置する :file:`src/main` ディレクトリと、アプリケーションのテスト用のコードを配置する :file:`src/test` ディレクトリに分かれます。

それぞれのディレクトリ/ファイルの構成を以下に示します。

..  list-table:: プロジェクトレイアウト - :file:`src/main`
    :widths: 4 6
    :header-rows: 1

    * - ファイル/ディレクトリ
      - 説明
    * - :file:`src/main/java`
      - Asakusa DSLのソースディレクトリ [#]_
    * - :file:`src/main/resources`
      - プロジェクトのリソースディレクトリ
    * - :file:`src/main/dmdl`
      - DMDLスクリプトディレクトリ
    * - :file:`src/main/libs`
      - プロジェクトの依存ライブラリディレクトリ [#]_

..  [#] ソースディレクトリ配下に配置するAsakusa DSLの推奨パッケージ名について、 :doc:`../dsl/start-guide` に記載されています。

..  [#] このディレクトリ内に直接配置したライブラリファイル(:file:`.jar`)のみ、バッチアプリケーション内で利用可能です。
        サブディレクトリに配置したライブラリファイルは無視されます。
        詳しくは、後述の  `ユーザー演算子で使用するライブラリの追加`_  を参照してください。

..  list-table:: プロジェクトレイアウト - :file:`src/test`
    :widths: 5 5
    :header-rows: 1

    * - ファイル/ディレクトリ
      - 説明
    * - :file:`src/test/java`
      - Asakusa DSLのテスト用ソースディレクトリ
    * - :file:`src/test/resources`
      - プロジェクトのテスト用リソースディレクトリ
    * - :file:`src/test/resources/logback-test.xml`
      - ビルド/テスト実行時に使用されるログ定義ファイル

ビルドディレクトリ
~~~~~~~~~~~~~~~~~~

プロジェクトのビルドディレクトリはGradleの各プラグインが提供するタスクの実行に対応したビルド成果物が作成されます。
デフォルト設定のビルドディレクトリは :file:`build` です。

ビルドディレクトリの主なディレクトリ/ファイルの構成を以下に示します [#]_ 。

..  list-table:: プロジェクトレイアウト - :file:`build`
    :widths: 3 2 5
    :header-rows: 1

    * - ファイル/ディレクトリ
      - 生成タスクの例
      - 説明
    * - :file:`generated-sources/modelgen`
      - :program:`compileDMDL`
      - DMDLコンパイラによって生成されるデータモデルクラス用ソースディレクトリ
    * - :file:`generated-sources/annotations`
      - :program:`classes`
      - Operator DSLコンパイラによって生成される演算子実装クラス/演算子ファクトリクラス用ソースディレクトリ
    * - :file:`excel`
      - :program:`generateTestbook`
      - テストデータ定義シートを生成するディレクトリ
    * - :file:`reports/tests`
      - :program:`check`
      - テストレポートファイルを生成するディレクトリ
    * - :file:`spark-batchapps`
      - :program:`compileBatchapp`
      - Sparkコンパイラによるバッチアプリケーション生成ディレクトリ
    * - :file:`*-batchapp-*.jar`
      - :program:`jarBatchapp`
      - バッチアプリケーションアーカイブファイル
    * - :file:`asakusafw-*.tar.gz`
      - :program:`assemble`
      - デプロイメントアーカイブファイル

..  [#] 各タスクが処理過程で生成するワークディレクトリについては割愛しています。
        また、ここで示すディレクトリ以外にも、実行するGradleのタスクによって様々なディレクトリが生成されます。

基本的なプラグインの使用方法
============================

ここでは、Asakusa Frameworkの開発の流れに沿ってAsakusa Gradle Plugin の基本的な使い方を紹介します。

以降の説明では、コマンドライン上のカレントディレクトリがサンプルアプリケーションを配置したディレクトリに設定されていることを前提とします。

..  code-block:: sh

    cd ~/workspace/example-basic-spark

開発用のAsakusa Frameworkインストール
-------------------------------------

Asakusa Frameworkを開発環境にインストールします。

Asakusa Frameworkを開発環境にインストールするには、インストールディレクトリパスを環境変数 ``ASAKUSA_HOME`` に定義した上で :program:`installAsakusafw` タスクを実行します。

プロジェクト上でタスクを実行するには、以下のように :program:`gradlew` コマンドにタスク名を指定して実行します。

..  code-block:: sh

    ./gradlew installAsakusafw

このタスクは ``ASAKUSA_HOME`` のパス上に開発環境用の構成を持つAsakusa Frameworkをインストールします。

..  attention::
    開発環境では、Asakusa DSLを使ってアプリケーションを記述するだけであればAsakusa Frameworkのインストールは不要ですが、テストドライバーを使ってFlow DSL、Batch DSLのテストを行う場合や、YAESSを使ってローカル環境でバッチアプリケーションを実行する場合など、Hadoopを実際に動作させる機能については、Asakusa Frameworkをインストールする必要があります。

データモデルクラスの生成
------------------------

DMDLスクリプトから演算子の実装で使用するデータモデルクラスを生成します。
DMDLスクリプトの記述や配置方法については :doc:`../dmdl/index` を参照してください。

データモデルクラスを生成するには、 :program:`compileDMDL` タスクを実行します。

..  code-block:: sh

    ./gradlew compileDMDL

このタスクはDMDLコンパイラを実行し、DMDLスクリプトディレクトリ( :file:`src/main/dmdl` )配下のDMDLスクリプトからデータモデルクラスをデータモデルクラス用ソースディレクトリ( :file:`build/generated-sources/modelgen` )配下に生成します。

データモデルクラスに使われるJavaパッケージ名は、ビルドスクリプト( :file:`build.gradle` ) の ``group`` で指定している値に ``.modelgen`` を加えた文字列になります。
プロジェクトテンプレートに含まれるビルドスクリプトの初期値は ``com.example`` となっているため、アプリケーションが使用する適切なパッケージ名に変更してください。

..  attention::
    DMDLスクリプトでモデル名を変更した後に :program:`compileDMDL` タスクを実行した場合、モデル名を変更する前のデータモデルクラスが出力ディレクトリに残ります。
    データモデルクラス用のソースディレクトリを初期化する場合、:program:`cleanCompileDMDL` タスクを合わせて実行します。

..  seealso::
    ``group`` と データモデルクラスのパッケージの文字列を個別に設定することも可能です。
    詳しくは後述の :ref:`gradle-plugin-customize` で説明します。

.. _batch-compile-gradle-plugin:

バッチアプリケーションのコンパイル
----------------------------------

Batch DSLコンパイラを使ってバッチアプリケーションのコンパイルを行い、実行可能モジュールを生成します。
Asakusa DSLの記述や配置方法については :doc:`../dsl/index` を参照してください。

バッチアプリケーションのコンパイルを行うには、 :program:`compileBatchapp` タスクを実行します。

..  code-block:: sh

    ./gradlew compileBatchapp

このタスクは、ビルドスクリプトに適用されているプラグイン構成に従って、利用するBatch DSLコンパイラを実行します。
例えばAsakusa on Sparkのプロジェクトテンプレートに含まれるビルドスクリプトの構成ではSpark向けのDSLコンパイラが実行されます。

その他、バッチアプリケーションのコンパイルでは以下のようなタスクが利用できます。

..  list-table:: バッチアプリケーションのコンパイルに関連するタスク
    :widths: 2 5
    :header-rows: 1

    * - タスク
      - 説明
    * - :program:`compileBatchapp`
      - プロジェクトのプラグイン構成に従って、それぞれのBatch DSLコンパイラを実行する [#]_ 。
    * - :program:`mapreduceCompileBatchapps`
      - MapReduce向けのDSLコンパイラを実行し、 :file:`build/batchc` 配下にコンパイル済みのバッチアプリケーションを配置する。
        このタスクを実行するにはプロジェクトのプラグイン構成に :doc:`../mapreduce/index` を含める必要がある。
    * - :program:`sparkCompileBatchapps`
      - Spark向けのDSLコンパイラを実行し、 :file:`build/spark-batchapps` 配下にコンパイル済みのバッチアプリケーションを配置する。
        このタスクを実行するにはプロジェクトのプラグイン構成に :doc:`../spark/index` を含める必要がある。
    * - :program:`m3bpCompileBatchapps`
      - |M3BP_ENGINE| 向けのDSLコンパイラを実行し、 :file:`build/m3bp-batchapps` 配下にコンパイル済みのバッチアプリケーションを配置する。
        このタスクを実行するにはプロジェクトのプラグイン構成に :doc:`../m3bp/index` を含める必要がある。
    * - :program:`jarBatchapp`
      - :program:`compileBatchapp` タスクで生成したバッチアプリケーションを含むjarファイルを生成し ``build/${project}-batchapps.jar`` に配置します。

..  [#] 例えばAsakusa on Sparkのプロジェクトテンプレートの初期構成では :program:`compileBatchapp` は :program:`sparkCompileBatchapps` を実行します。

デプロイメントアーカイブの構成
------------------------------

Asakusa Frameworkのバッチアプリケーションをアプリケーション運用環境（Hadoopクラスターなど）で実行するには、DSLコンパイルによって作成したバッチアプリケーションとAsakusa Framework本体の実行モジュールをあわせて運用環境にデプロイします。

Asakusa Gradle Pluginでは運用環境にデプロイする必要がある実行モジュールを全て含めた「デプロイメントアーカイブ」と呼ばれるパッケージファイルを生成することができます。

デプロイメントアーカイブの作成には、:program:`assemble` タスクを実行します。

..  code-block:: sh

    ./gradlew assemble

:program:`assemble` タスクを実行すると、 :file:`build` ディレクトリ配下に ``asakusafw-${project.name}.tar.gz`` というファイル名でデプロイメントアーカイブファイルが生成されます。

デプロイメントアーカイブファイルは運用環境上の ``$ASAKUSA_HOME`` 配下に展開してデプロイします。
運用環境へのデプロイメントや :program:`assemble` タスクの具体的な使用例については、 :doc:`../administration/deployment-guide` を参照してください。

..  seealso::
    デプロイメントアーカイブファイル名やファイルに含まれる内容を個別に設定することも可能です。
    詳しくは後述の :ref:`gradle-plugin-customize` で説明します。

アプリケーションのテスト
------------------------

Asakusa DSLとして記述したバッチアプリケーションに対して、テストロジックを実行してテストを行います。
Asakusa DSLのテスト手法については、 :doc:`../testing/index` などを参照してください。

Asakusa DSLのテストを実行するには、 :program:`check` タスクを実行します。

..  code-block:: sh

    ./gradlew check

:program:`check` タスクを実行すると、Asakusa DSLのテスト用ソースディレクトリ( :file:`src/test/java` )に含まれるテストクラスを自動的に検出し、これを実行します。

テストの実行結果は :file:`build/reports/tests` 配下にHTML形式のテストレポートが生成されます。
また、 :file:`build/test-results` にはXML形式のテスト結果ファイルが生成されます。
このXMLファイルはCIサーバーなどのツールと連携して使用することができます。

テストドライバーの :doc:`../testing/using-excel` を使用したテストを記述する場合、 :program:`generateTestbook` タスクを実行することでテストデータ定義シート（テストデータテンプレート）を生成することができます。

..  code-block:: sh

    ./gradlew generateTestbook

:program:`generateTestbook` タスクを実行すると、 :file:`build/excel` 配下にDMDLで記述したデータモデルに対応するテストデータ定義シートが作成されます。

フルビルド
----------

:program:`build` タスクはプロジェクトのフルビルドを実行します。
実際には上記 :program:`assemble` と :program:`check` タスクを実行します。
CIサーバーなどでリリースビルドを行うような場合に、このタスクを利用するとよいでしょう。

..  code-block:: sh

    ./gradlew clean build

..  hint::
    :program:`clean` タスクはプロジェクトのビルドディレクトリ ( :file:`build` )を初期化します。
    リリースビルドを行うような場合は合わせて実行するとよいでしょう。

.. _gradle-plugin-using-eclipse:

Eclipse定義ファイルの作成
-------------------------

アプリケーション開発用の統合開発環境(IDE)にEclipseを使用する場合、開発環境にEclipseをインストールした上で、プロジェクトに対してEclipseプロジェクト用の定義ファイルを追加します。

Eclipseプロジェクト用の定義ファイルを作成するには、 :program:`eclipse` タスクを実行します。

..  code-block:: sh

    ./gradlew eclipse

このコマンドを実行することによって、プロジェクトディレクトリに対してEclipseプロジェクト用の定義ファイルやEclipse上のクラスパスに対応したソースディレクトリなどが追加されます。
これにより、Eclipseからプロジェクトをインポートすることが可能になります。

..  tip::
    Eclipseからプロジェクトをインポートするには、Eclipseのメニューから :menuselection:`File --> Import --> General --> Existing Projects into Workspace` を選択し、プロジェクトディレクトリを指定します。

.. _gradle-plugin-customize:

ビルド設定のカスタマイズ
========================

ビルドに関する設定をカスタマイズするには、基本的にはGradleのビルドスクリプトである :file:`build.gradle` を編集します。

以下は、いくつかの基本的なカスタマイズをおこなったビルドスクリプトの例です。

..  literalinclude:: gradle-attachment/custom-build.gradle
    :language: groovy
    :caption: build.gradle
    :name: build.gradle-gradle-plugin-2

標準プロジェクトプロパティ
--------------------------

標準的なプロジェクト情報は、以下のようにビルドスクリプトのトップレベルの階層に定義します。

..  literalinclude:: gradle-attachment/custom-build.gradle
    :language: groovy
    :caption: build.gradle
    :name: build.gradle-gradle-plugin-3
    :lines: 1-4

``group`` プロパティはプラグインの各タスクでJavaソースコードの生成時に指定する基底Javaパッケージとして使用されます。

``version`` プロパティはアーカイブファイル名に付加されたり、バッチアプリケーションのコンパイル時のビルド情報ファイルに含まれます。

指定可能なプロパティ一覧についてはGradleのドキュメントを参照してください。

プラグイン規約プロパティ
------------------------

Asakusa Gradle Pluginが提供する各タスクの動作に関する設定は、プラグイン規約プロパティに設定します。

Asakusa Gradle Pluginのプラグイン規約プロパティは、以下に説明するビルドスクリプトの各ブロックに設定します。

``asakusafw`` ブロックの設定
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``asakusafw`` ブロックにはプロジェクト内で開発、管理するバッチアプリケーションに関する設定情報を指定します。

``asakusafw`` ブロックは設定のカテゴリ別にさらに階層化されています。

以下の例では、プロジェクトで使用するコード自動生成用の規定パッケージ名 を ``basePackage`` で指定し、続いてSpark向けのDSLコンパイルの設定に関する ``spark`` ブロックが指定されています。
ブロック内には複数のプロパティを指定することができます。

..  literalinclude:: gradle-attachment/custom-build.gradle
    :language: groovy
    :caption: build.gradle
    :name: build.gradle-gradle-plugin-4
    :lines: 19-24

..  seealso::
    ``asakusafw`` ブロックの設定に関する機能は、Asakusa Gradle Pluginに含まれる Batch Application Plugin が提供します。
    Batch Application Pluginに関する説明や、 ``asakusafw`` ブロックで設定可能なプロパティについては :doc:`gradle-plugin-reference` - :ref:`batch-application-plugin-reference` を参照してください。

``asakusafwOrganizer`` ブロックの設定
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``asakusafwOrganizer`` ブロックには開発環境や運用環境の構成に関する設定情報を指定します。

``asakusafwOrganizer`` ブロックは設定のカテゴリ別にさらに階層化されています。

以下の例では、WindGateに対してリトライ処理を有効にする拡張コンポーネントを追加しています。

``profiles`` から始まるブロックは、デプロイメントアーカイブの構成情報を管理するプロファイルに関する設定です。
プロファイルについては後述の `プロファイルの管理`_ で詳しく説明します。

..  literalinclude:: gradle-attachment/custom-build.gradle
    :language: groovy
    :caption: build.gradle
    :name: build.gradle-gradle-plugin-5
    :lines: 26-33

..  seealso::
    ``asakusafwOrganizer`` ブロックの設定に関する機能は、Asakusa Gradle Pluginに含まれる Framework Organizer Plugin が提供します。
    Framework Organizer Pluginに関する説明や、 ``asakusafwOrganizer`` ブロックで設定可能なプロパティについては :doc:`gradle-plugin-reference` - :ref:`framework-organizer-plugin-reference` を参照してください。

.. _gradle-plugin-dependency-management:

依存ライブラリの管理
====================

ここではアプリケーションプロジェクトが利用する拡張機能や追加ライブラリなどの依存ライブラリを管理する方法を説明します。

アプリケーションSDKライブラリ
-----------------------------

Asakusa Frameworkではアプリケーションの開発で使用するAsakusa Frameworkのライブラリをグループ化した「アプリケーションSDKライブラリ」を提供しています。

Asakusa Gradle Pluginを適用したアプリケーションプロジェクトでは、標準的なアプリケーションSDKライブラリがプロジェクトの依存ライブラリに自動的に追加されます。

標準の設定では無効化されている試験的機能を有効にしたり、利用しないSDKライブラリを無効化するなどの場合には、アプリケーションSDKライブラリの設定を変更します。

アプリケーションSDKライブラリは、 ビルドスクリプトの ``asakusafw`` ブロック配下の ``sdk`` で指定します。

以下の例では、プロジェクトテンプレートに含まれるビルドスクリプトに対して :doc:`../directio/using-hive` を利用するためのアプリケーションSDKライブラリを追加しています。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-gradle-plugin-6

    asakusafw {
        sdk.hive true
    }

..  seealso::
    ``sdk`` に対して設定可能な全てのプロパティについては :doc:`gradle-plugin-reference` - :ref:`batch-application-plugin-reference` を参照してください。

.. _dependency-library-gradle-plugin:

ユーザー演算子で使用するライブラリの追加
----------------------------------------

バッチアプリケーションのユーザー演算子から任意のJavaライブラリを利用する場合は、以下に示すいずれかの方法でアプリケーション用依存ライブラリを追加します。

プロジェクトの依存ライブラリディレクトリへjarファイルを配置
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

プロジェクトディレクトリの「依存ライブラリディレクトリ」( :file:`src/main/libs` ) 配下にjarファイルを配置すると、Javaソースファイルのコンパイル時にこのライブラリが依存関係に追加され、さらにDSLコンパイルの結果バッチアプリケーションの実行ファイルに自動的に含まれるようになります。

通常はこの方法でライブラリを追加することを推奨します。

..  attention::
    :file:`src/main/libs` ディレクトリの直下に配置したjarファイルのみ有効です。
    サブディレクトリを作成してその中にjarファイルを配置してもそのファイルは無視されます。

Asakusaの拡張ライブラリディレクトリへjarファイルを配置
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

バッチアプリケーションの実行時に依存ライブラリを利用するもう一つの方法は、Asakusa Framework全体の「拡張ライブラリディレクトリ」( :file:`$ASAKUSA_HOME/ext/lib` )に対象のjarファイルを直接配置します。
拡張ライブラリディレクトリに追加したjarファイルは、実行時に全てのバッチアプリケーションから参照できます。

この場合、Javaソースのコンパイル時にはこのライブラリは参照されないため、ビルドスクリプトの ``dependencies`` ブロックにも依存関係の追加を行う必要があることに注意してください。

Eclipse設定の更新
~~~~~~~~~~~~~~~~~

Eclipseを使用している場合は、上記の方法で依存ライブラリを追加した後に、Eclipseプロジェクト上のクラスパス設定を更新する必要があります。

以下は更新の実行例です。

..  code-block:: sh

    ./gradlew cleanEclipse eclipse

.. _gradle-plugin-oraganizer-profile:

プロファイルの管理
==================

Asakusa Gradle Pluginでは、特定の環境向けに個別にデプロイメントアーカイブの構成を設定するために「プロファイル」を定義することができます。

プロファイルの指定は、 ``asakusafwOrganizer`` ブロック内の参照名 ``profiles`` で定義します [#]_ 。プロファイルは複数設定することが可能です。
プロファイルを定義することで、 :program:`assemble` タスクの実行時（後述の ``dev`` プロファイルを除く）にプロファイルごとの各設定に従ったデプロイメントアーカイブを生成します。

標準では、以下のプロファイルが設定されています。

..  list-table:: 標準プロファイル
    :widths: 2 8
    :header-rows: 1

    * - プロファイル名
      - 説明
    * -  ``dev``
      - 開発環境向けのデプロイ構成を定義するプロファイル
    * -  ``prod``
      - 運用環境向けのデプロイ構成を定義するプロファイル

``dev`` プロファイルは開発環境向けに用意された特殊なプロファイルです。テストドライバー用の構成が有効になるなど、開発環境向けの既定値が設定されています。他のプロファイルとは異なり、 :program:`installAsakusafw` タスクの実行時に本プロファイルの設定が使用されます。

標準で設定されているプロファイルに加えて、 ``asakusafwOrganizer`` ブロック配下に ``profiles.<profile-name>`` という形式で任意のプロファイルを追加することができます。

以下は、ステージング環境用のデプロイ構成を持つプロファイル ``stage`` を定義する例です。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-gradle-plugin-8

    asakusafwOrganizer {
        hive.enabled true
        windgate.retryableEnabled true
        profiles.prod {
            assembly.into('.') {
                put 'src/dist/prod'
            }
        }
        profiles.stage {
            archiveName 'asakusa-dist-stage.tar.gz'
            assembly.into('.') {
                put 'src/dist/stage'
            }
        }
    }

デプロイメントアーカイブの生成を行うと、 ``build`` ディレクトリ配下に ``asakusafw-${project.name}-<profile-name>.tar.gz`` というファイル名 [#]_ で プロファイルに対応したデプロイメントアーカイブが生成されます。

プロファイル内では上記で説明したコンポーネントごとの規約プロパティや ``assembly`` プロパティを使ったデプロイメントアーカイブの編集機能を使うことができます。

``asakusafwOrganizer`` 直下に設定されている設定は、すべてのプロファイルに反映されます。
同じプロパティが ``asakusafwOrganizer`` 直下とプロファイルの両方に指定されていた場合は、プロファイル側の設定が利用されます。

例えば上の例では、 ``asakusafwOrganizer`` 直下の ``hive.enabled true`` の設定はプロファイル ``prod`` と ``stage`` 、及び標準のプロフィルである ``dev`` に反映されます。

..  [#] これらの機能は :asakusa-gradle-groovydoc:`com.asakusafw.gradle.plugins.AsakusafwOrganizerProfile` が提供します。
..  [#] 標準の設定では、プロファイル ``prod`` のデプロイメントアーカイブは ``asakusafw-${project.name}.tar.gz`` というファイル名(プロファイル名が接尾辞につかない)で生成されます。
        上記の ``stage`` プロファイルの例のように、プロパティ ``archiveName`` を設定することで任意のファイル名を指定することもできます。

..  attention::
    ``dev`` というプロファイル名は開発環境向けのプロファイルとして予約されているため、運用環境向けのプロファイルとしては利用できないことに注意してください。

..  seealso::
    ``asakusafwOrganizer`` ブロック や ``profiles`` ブロック内で利用可能な設定項目については :doc:`gradle-plugin-reference` - :ref:`framework-organizer-plugin-reference` を参照してください。

..  seealso::
    プロファイルの設定例や、デプロイメントアーカイブを使った運用環境へのデプロイについては、:doc:`../administration/deployment-guide` も参照してください。

その他のプラグイン機能
======================

ここでは、Asakusa Gradle Pluginの利用方法をいくつか紹介します。
より詳しい情報は、:doc:`gradle-plugin-reference` や各タスクのGroovyDocを参照してください。

プラグイン構成の追加
--------------------

プロジェクトテンプレートの初期構成に対して、プラグイン構成を後から追加、削除することができます。
プラグイン構成を追加するには、ビルドスクリプト上に ``apply plugin: <プラグインID>`` のように利用するプラグイン構成を追加します。

以下の設定例では、Asakusa on Spark向けのプロジェクトテンプレートに対して、 |M3BP_FEATURE| 用のプラグイン ``asakusafw-m3bp`` を追加しています。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-gradle-plugin-9

    apply plugin: 'asakusafw-sdk'
    apply plugin: 'asakusafw-organizer'
    apply plugin: 'asakusafw-spark'
    apply plugin: 'asakusafw-m3bp'
    apply plugin: 'eclipse'

..  seealso::
    Asakusa Gradle Pluginが提供するプラグインの一覧は :doc:`gradle-plugin-reference` を参照してください。

..  _gradle-plugin-dslcompile-disable:

デプロイメント構成に対するDSLコンパイラの無効化
-----------------------------------------------

ビルドスクリプトの構成で有効となっているDSLコンパイラに対して、 デプロイメント構成ごとにDSLコンパイラの生成物を含めないよう設定することができます。

以下の設定例では、すべてのデプロイメント構成に対して |M3BP_FEATURE| コンパイラを無効にしています。
この設定により、 :program:`assemble` タスクの実行時に |M3BP_FEATURE| コンパイラのコンパイル処理がスキップされます。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-gradle-plugin-10

    asakusafwOrganizer {
        spark.enabled true
        m3bp.enabled false
    }

これらの記述を省略した場合、各DSLコンパイラの利用は有効 ( ``true`` ) になります。

これらの設定はプロファイル単位でも設定することができます。

..  _gradle-plugin-dslcompile-filter:

バッチコンパイル対象のフィルタリング
------------------------------------

標準の構成では、バッチアプリケーションのコンパイルやデプロイメントの構成時にはプロジェクトに含まれる全てのバッチクラスがコンパイルの対象となりますが、
ビルドスクリプトの定義やコマンドラインの指定で、一部のバッチクラスのみをコンパイルすることができます。

ビルドスクリプトの指定によるフィルタリング
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ビルドスクリプトの構成で有効となっている各DSLコンパイラに対して、コンパイル対象のバッチクラスを include/exclude することができます。

以下、ビルドスクリプトの設定例です。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-gradle-plugin-11

    asakusafw {
        spark {
            exclude 'com.example.batch.Hoge'
        }
        m3bp {
            include 'com.example.batch.Hoge'
        }
    }

上記の設定例では、Asakusa on Sparkコンパイラに対してはバッチクラス ``com.example.batch.Hoge`` を除外しその他のバッチクラスを全て含めるよう指定しています。
また |M3BP_FEATURE| コンパイラに対してはバッチクラス ``com.example.batch.Hoge`` のみを含めるよう指定しています。

バッチクラス名の文字列には ``*`` をワイルドカードとして使用することもできます。

コマンドラインの指定によるフィルタリング
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

コマンドラインの指定によるフィルタリングは、ビルドスクリプトの設定に対してコマンドライン上でバッチコンパイルの対象をフィルタリングします。

コマンドラインの指定によるフィルタリングは、:program:`gradlew` コマンドを実行する際に各DSLコンパイラの実行用タスクの後に ``--update <バッチクラス名>`` と指定します。
例えば、Asakusa on Sparkコンパイラに対してフィルタリングを指定するには、 ``sparkCompileBatchapps --update <バッチクラス名>`` と指定します。

バッチクラス名の文字列には ``*`` をワイルドカードとして使用することもできます。

以下の例では、パッケージ名に ``com.example.target.batch`` を含むバッチクラスのみをバッチコンパイルしてデプロイメントアーカイブを作成しています。

..  code-block:: sh

    ./gradlew sparkCompileBatchapps --update com.example.target.batch.* assemble

..  attention::
    Asakusa Frameworkのバージョン ``0.7.6`` 以前では :program:`compileBatchapp` タスクに対して ``--update`` オプションを指定していましたが、
    バージョン ``0.8.0`` 以降は :program:`compileBatchapp` タスクに ``--update`` オプションを指定することはできなくなりました。
    代わりに、各DSLコンパイラの実行用タスク ( :program:`sparkCompileBatchapps` など ) に ``--update`` オプションを指定します。

テストツールタスクの実行
------------------------

..  experimental::
    Asakusa Framework バージョン |version| では、 ``TestTookTask`` は試験的機能として提供しています。

``TestToolTask`` [#]_ を使うことで、テストドライバーやバッチテストランナーが持つ機能を組み合わせてGradleのタスクとして実行することができます。

..  seealso::
    テストツールタスクの利用例は :doc:`../testing/user-guide` - :ref:`testing-userguide-testtool-task` を参照してください。

..  [#] :asakusa-gradle-groovydoc:`com.asakusafw.gradle.tasks.TestToolTask`

.. _gradle-plugin-task-hiveddl:

Hive用DDLファイルの生成
-----------------------

:program:`generateHiveDDL` は Hive連携用の拡張属性を持つDMDLスクリプトからをHive用のDDLファイルを生成します。

:program:`generateHiveDDL` タスクを実行すると、プロジェクトの :file:`build/hive-ddl` ディレクトリ配下にHiveのテーブル作成用の ``CREATE TABLE`` 文を含むSQLファイルが生成されます。

:program:`generateHiveDDL` タスクは :program:`gradlew` コマンド実行時に以下のコマンドラインオプションを指定することができます。


..  program:: generateHiveDDL

..  option:: --location /path/to/base-location

    生成する ``CREATE TABLE`` 文に ``LOCATION`` (テーブルに対応するファイルを配置するHDFS上のパス) を追加する
    (``LOCATION`` の値に ``'<指定したパス>/<table-name>'`` を追加)

    指定がない場合は ``LOCATION`` 句は未指定

..  option:: --database-name database-name

    生成する ``CRATE TABLE`` 文のテーブル名の前にデータベース名を付与する

    指定がない場合は データベース名は未指定

..  option:: --include regex-table-name-pattern

    指定した正規表現にマッチするテーブルに対してのみDDLを生成する

    指定がない場合はすべてのテーブルに対してDDLを生成する

..  option:: --output /path/to/ddloutput

    指定した出力先のパスにDDLファイルを生成する

    指定がない場合のファイルパスは ``${project.buildDir}/hive-ddl/${project.name}.sql``

:program:`generateHiveDDL` タスクの実行例は以下の通りです。

..  code-block:: sh

    ./gradlew generateHiveDDL --location /home/hadoop/target/testing/directio/tables --include item

..  seealso::
    Hiveとの連携については、 :doc:`../directio/using-hive` を参照してください。
