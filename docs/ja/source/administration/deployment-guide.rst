======================================
Asakusa Framework デプロイメントガイド
======================================

この文書は、運用環境に対してAsakusa Frameworkの実行環境をセットアップする方法に関して説明します。

..  seealso::
    :basic-tutorial:`Asakusa Framework チュートリアル <index.html>` の後半にも実行環境のセットアップについて説明しています。

運用環境の準備
==============

まず、Asakusa Frameworkの実行環境をセットアップするための運用環境を準備します。

Asakusa on MapReduce / Asakusa on Spark 実行環境
------------------------------------------------

Asakusa on MapReduce や Asakusa on Spark を利用する場合には、通常は運用環境にHadoopクラスターを用意します。

Hadoopクラスターの具体的な構築手順は、利用するHadoopディストリビューションのドキュメント等を参考にして下さい。

また、SparkのセットアップはHadoopディストリビューションが提供するSparkを利用する方法や、
`Apache Sparkのダウンロードサイト`_ からSparkの実行バイナリを入手してセットアップする方法などがあります。

..  _Apache Sparkのダウンロードサイト: http://spark.apache.org/downloads.html

|M3BP_FEATURE| 実行環境
-----------------------

|M3BP_FEATURE| を利用する場合には、 :doc:`../m3bp/user-guide` - :ref:`m3bp-target-platform` の「実行環境」に記載する構成を持つ環境を準備します。

..  hint::
    Red Hat Enterprise Linux 7.2 (またはCentOS 7.2) を利用する場合、OSの初期セットアップの方法にもよりますが多くの場合
    GNU C++ Library 及び GNU C Library は初期構成に含まれます。

    また、Portable Hardware Localityを追加するコマンド例は以下の通りです。

    ..  code-block:: sh

        yum install hwloc

Asakusa Frameworkのデプロイメント
=================================

ここでは、運用環境にAsakusa Frameworkをデプロイしてバッチアプリケーションの実行を行うまでの手順を、以下のトピックに分けて説明します。

* `デプロイメントアーカイブの作成`_
* `デプロイメントアーカイブの配置`_
* `環境構成の確認とバッチの実行`_

以降の説明では、Akakusa Frameworkの開発環境でアプリケーションプロジェクトからバッチアプリケーションが作成できることを前提とします。
開発環境の構築やアプリケーションプロジェクトについては、 :doc:`../introduction/start-guide` や :doc:`../application/gradle-plugin` などを参照してください。

デプロイメントアーカイブの作成
------------------------------

Asakusa Frameworkを運用環境で利用するには、Asakusa Frameworkの「実行モジュール」一式を運用環境に配置する必要があります。

実行モジュールには、たとえば以下のようなものが含まれます。

* Asakusa Framework本体の実行ライブラリ
* Asakusa Frameworkで生成したバッチアプリケーション
* Asakusa Frameworkのプラグインモジュール
* Asakusa Frameworkの設定ファイル

Asakusa Frameworkではこれらの実行モジュールを生成する様々な方法を提供していますが、ここでは運用環境への配置が必要な全てのファイルを「デプロイメントアーカイブ」と呼ばれる単一のアーカイブファイルに含める方法について、いくつかの構成例とともに説明します。

シンプルな例
~~~~~~~~~~~~

デプロイメントアーカイブの作成には、Gradleの :program:`assemble` タスクを実行します。

..  code-block:: sh

    ./gradlew assemble

アプリケーションプロジェクトの標準設定でGradleの :program:`assemble` タスクを実行すると、バッチアプリケーションのビルドが行われ、ビルドが成功した場合はデプロイメントアーカイブがプロジェクトの :file:`build` ディレクトリ配下に ``asakusafw-${project.name}.tar.gz`` というファイル名で生成されます。

このデプロイメントアーカイブには以下のモジュールが含まれます。

* Asakusa Framework本体の実行ライブラリ
* プロジェクトに含まれるすべてのバッチアプリケーション
* Asakusa Frameworkの標準設定の設定ファイル

プラグイン構成の追加
~~~~~~~~~~~~~~~~~~~~

プロジェクトテンプレートの初期構成に対して、プラグイン構成を後から追加、削除することができます。
プラグイン構成を追加するには、ビルドスクリプト上に ``apply plugin: <プラグインID>`` のように利用するプラグイン構成を追加します。

以下の設定例では、Asakusa on Spark向けのプロジェクトテンプレートに対して、 Asakusa on MapReduce用のプラグイン ``asakusafw-mapreduce`` を追加しています。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-deployment-guide-6

    apply plugin: 'asakusafw-sdk'
    apply plugin: 'asakusafw-organizer'
    apply plugin: 'asakusafw-spark'
    apply plugin: 'asakusafw-mapreduce'
    apply plugin: 'eclipse'

この状態で :program:`assemble` タスクを実行すると、Spark環境向けのバッチアプリケーションとMapReduce環境向けのバッチアプリケーションが生成されます。

設定ファイルの同梱
~~~~~~~~~~~~~~~~~~

デプロイメントアーカイブに、特定の運用環境向けの設定ファイルを含めることもできます。

以下は、 プロジェクトディレクトリの :file:`src/dist/prod` 配下に配置した設定ファイルをデプロイメントアーカイブに含める例です。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-deployment-guide-1

    asakusafwOrganizer {
        profiles.prod {
            assembly.into('.') {
                put 'src/dist/prod'
            }
        }
    }

デプロイメントアーカイブの構成を変更するには、アプリケーションプロジェクトのビルドスクリプト :file:`build.gradle` の ``asakusafwOrganizer`` ブロックを編集します。

また、標準のデプロイメント構成用のプロファイルである ``profile.prod`` ブロック内の設定を編集します。
プロファイルについては後述の `複数の運用環境向けのデプロイ管理`_ にて説明します。

``assembly.into`` は引数に指定したパス上にファイルを含めることを意味します。
例では引数に ``(.)`` と記述しており、これはデプロイメントアーカイブのルートディレクトリに対してファイルを含める指定となります。

``assembly.into`` ブロック配下の ``put`` の引数で含める対象となるファイルパスを指定します。
相対パスで指定した場合はプロジェクトディレクトリが起点となります。
この例では、 :file:`src/dist/prod` 配下には ``$ASAKUSA_HOME`` のディレクトリ構造と同じ形式で追加したい設定ファイルを以下のように配置しています。

..  code-block:: sh

    src/dist/prod
     ├── core
     │   └── conf
     │       └── asakusa-resources.xml
     └── yaess
         └── conf
             └── yaess.properties

``asakusafwOrganizer`` ブロック上では上記の他にも様々な構成に関する設定が可能です。
詳しくは :doc:`../application/gradle-plugin` や :doc:`../application/gradle-plugin-reference` を参照してください。

以下は、 ``asakusafwOrganizer`` ブロック に対するいくつかの設定例を紹介します。

拡張モジュールの同梱
~~~~~~~~~~~~~~~~~~~~

Asakusa Frameworkが標準のデプロイメントアーカイブに含めていない拡張モジュールを同梱する例です。

以下は、Direct I/O Hive用の実行ライブラリとWindGateのリトライ実行用の拡張プラグイン ``asakusa-windgate-retryable`` をデプロイメントアーカイブに含める例です。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-deployment-guide-2

    asakusafwOrganizer {
        hive.enabled true
        windgate.retryableEnabled true
    }

この例では、設定を ( ``profiles.prod`` ブロックではなく ) ``asakusafwOrganizer`` ブロックの直下に追加しているため、この設定は開発環境用のインストール構成にも適用されます。

..  seealso::
    拡張モジュールの利用については、 :doc:`../application/gradle-plugin` を参照してください。

Hiveライブラリの指定
~~~~~~~~~~~~~~~~~~~~

開発環境と運用環境でDirect I/O Hive用の実行ライブラリを分ける例です。

以下は、運用環境のHadoopディストリビューションに `MapR`_ を利用する環境でDirect I/O Hiveを利用するための設定例です。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-deployment-guide-3

    repositories {
        maven { url 'http://repository.mapr.com/maven/' }
    }

    asakusafwOrganizer {
        hive.enabled true
        profiles.prod {
            hive.libraries = ['org.apache.hive:hive-exec-1.2.0-mapr-1609@jar']
        }
    }

Direct I/O Hiveを `MapR`_ 環境で利用する場合、Direct I/O HiveはMapRが提供するHiveライブラリを利用する必要があるため、運用環境用のデプロイメントアーカイブにHiveライブラリを指定しています。

..  hint::
    `MapR`_ 用のライブラリを指定する場合、MapRライブラリ配布用のMavenリポジトリを ``repositories`` ブロックで指定する必要があります。

..  seealso::
    Direct I/O Hiveについては、 :doc:`../directio/using-hive` を参照してください。

..  _`MapR`: https://www.mapr.com/

.. _deployment-extention-libraries-example:

外部ライブラリの配置
~~~~~~~~~~~~~~~~~~~~

外部ライブラリやAsakusa Frameworkが標準で同梱しない、特別な実行時プラグインを利用する場合、 :file:`$ASAKUSA_HOME/ext/lib` 配下にライブラリを配置すると利用可能になります [#]_ 。

これらのライブラリをデプロイメントアーカイブに同梱するには、以下のように書けます。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-deployment-guide-4

    asakusafwOrganizer {
        extension {
            libraries += ["com.asakusafw.sandbox:asakusa-directio-runtime-ext:${asakusafw.core.version}"]
        }
    }

..  attention::
    この機能では、指定したライブラリの推移的依存関係となるライブラリは含まれません。

..  hint::
    リポジトリ上に存在しないライブラリを同梱したい場合には、 `設定ファイルの同梱`_ と同様の方法でもライブラリファイルを配置することができます。

..  [#] 実行時プラグインの配置については、 :doc:`deployment-runtime-plugins` の内容も参照してください。

複数の運用環境向けのデプロイ管理
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

バッチアプリケーションを実行する運用環境が複数ある場合、環境ごとにデプロイ構成を変更したい場合があります。
このような場合、運用環境ごとにデプロイ構成用のプロファイルを作成すると便利です。

以下は、ステージング環境用のデプロイ構成を持つデプロイメントアーカイブを作成する例です。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-deployment-guide-5
    :emphasize-lines: 2,11

    asakusafwOrganizer {
        profiles.prod {
            assembly.into('.') {
                put 'src/dist/prod'
            }
            assembly.into('.') {
                put 'src/dist/common'
                replace 'asakusa-resources.xml', inputCombineMax: '48'
            }
        }
        profiles.stage {
            assembly.into('.') {
                put 'src/dist/stage'
            }
            assembly.into('.') {
                put 'src/dist/common'
                replace 'asakusa-resources.xml', inputCombineMax: '24'
            }
        }
    }

標準で設定されているプロファイル ``profiles.prod`` に加えて、ステージング環境用のプロファイルとして ``profiles.stage`` を追加しています。

この設定でデプロイメントアーカイブの生成を行うと、 :file:`build` ディレクトリ配下に標準のデプロイメントアーカイブに加えて、 ``asakusafw-${project.name}-stage.tar.gz`` というファイル名で ``profiles.stage`` に対応したデプロイメントアーカイブが生成されます。

この例では、それぞれのプロファイル用に作成した設定ファイル用のディレクトリ( :file:`src/dist/prod`, :file:`src/dist/stage` )から設定ファイルを配置しています。

また、それぞれのプロファイルに共通の設定ファイルを管理するディレクトリ( :file:`src/dist/common` )からも設定ファイルを配置しています。
このとき、 ``replace`` 句を指定することで設定ファイルの内容を置換して、環境ごとに固有のパラメータを設定しています。

例えば、 :file:`src/dist/common` 配下に :file:`asakusa-resources.xml` を以下のような内容で配置します。

..  code-block:: xml
    :caption: asakusa-resources.xml
    :name: asakusa-resources.xml-deployment-guide-1

    <?xml version="1.0" encoding="UTF-8"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <configuration>
        ...
        <property>
            <name>com.asakusafw.input.combine.max</name>
            <value>@inputCombineMax@</value>
        </property>
    </configuration>

:file:`build.gradle` では以下のように設定しているため、

* ``profile.prod`` ブロック: ``replace 'asakusa-resources.xml', inputCombineMax: '48'``
* ``profile.stage`` ブロック: ``replace 'asakusa-resources.xml', inputCombineMax: '24'``

それぞれのデプロイメントアーカイブの :file:`asakusa-resources.xml` にはこれらの設定値が置換された状態の設定ファイルが同梱されます。

..  seealso::
    :doc:`../sandbox/asakusa-on-emr` でも運用環境向けのデプロイ機能の利用例を紹介しています。デプロイ構成を柔軟に設定できるこれらの機能は、クラウド環境上で様々な運用環境を構築する場合などでも効果的でしょう。

デプロイメントアーカイブの配置
------------------------------

`デプロイメントアーカイブの作成`_ で作成したデプロイメントアーカイブを運用環境に配置します。

以降では、Asakusa on SparkとAsakusa on MapReduce向けのバッチアプリケーションを実行するために、Hadoopクラスターに配置する例を説明します。

また、ここでは運用環境上に構築したHadoopクラスターの各ノードうち、Asakusa Frameworkを配置してバッチアプリケーションの実行操作を行うノードを「Hadoopクライアントマシン」と呼びます。

環境変数の設定
~~~~~~~~~~~~~~

Hadoopクライアントマシン上でAsakusa Frameworkを配置しバッチアプリケーションの実行操作を行うOSユーザーに対して、以下の環境変数を設定します。

* ``JAVA_HOME``: YAESSが使用するJavaのインストールパス
* ``HADOOP_CMD``: YAESSが使用する :program:`hadoop` コマンドのパス
* ``SPARK_CMD``: YAESSが使用する :program:`spark-submit` コマンドのパス
* ``ASAKUSA_HOME``: Asakusa Frameworkのインストールパス

:file:`~/.profile` をエディタで開き、最下行に以下の定義を追加します。

..  code-block:: sh
    :caption: .profile
    :name: .profile-deployment-guide-1

    export JAVA_HOME=/usr/lib/jvm/java-8-oracle
    export HADOOP_CMD=/usr/lib/hadoop/bin/hadoop
    export SPARK_CMD=/opt/spark/bin/spark-submit
    export ASAKUSA_HOME=$HOME/asakusa

:file:`~/.profile` を保存した後、設定した環境変数をターミナル上のシェルに反映させるため、以下のコマンドを実行します。

..  code-block:: sh

    . ~/.profile

..  attention::
    実際に必要となる環境変数は利用するコンポーネントや構成によって異なります。
    これらの詳細はAsakusa Frameworkの各コンポーネントのドキュメントを参照してください

デプロイメントアーカイブの展開
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Hadoopクライアントマシンにデプロイメントアーカイブファイル ``asakusafw-${project.name}.tar.gz`` を配置し、 ``$ASAKUSA_HOME`` 配下にデプロイメントアーカイブを展開します。
展開後、 ``$ASAKUSA_HOME`` 配下の :file:`*.sh` に実行権限を追加します。
展開後 ``$ASAKUSA_HOME/tools/bin/setup.jar`` を実行し、展開したファイルに対して適切な実行権限などを設定します。


..  code-block:: sh

    mkdir -p "$ASAKUSA_HOME"
    cd "$ASAKUSA_HOME"
    tar -xzf /path/to/asakusafw-*.tar.gz
    java -jar $ASAKUSA_HOME/tools/bin/setup.jar

..  attention::
    ``$ASAKUSA_HOME/tools/bin/setup.jar`` を実行する手順は Asakusa Framework バージョン 0.10.0 より追加されました。

    バージョン 0.9以前では、この部分は ``find "$ASAKUSA_HOME" -name "*.sh" | xargs chmod u+x`` のようなコマンドを実行していましたが、
    バージョン 0.10.0 から展開後のファイル拡張子が変更されたため、この手順では正しくセットアップすることができません。

    そのためバージョン 0.10.0 以降は必ず上記のように ``$ASAKUSA_HOME/tools/bin/setup.jar`` を実行してセットアップを行ってください。

..  seealso::
    ``setup.jar`` の詳細は、 :doc:`utility-tool-user-guide` を参照してください。

環境構成の確認とバッチの実行
----------------------------

運用環境にデプロイしたAsakusa Frameworkのバッチアプリケーションを以下の手順で実行して運用環境上の動作確認を行います。

* 動作確認用テストデータの配置
* バッチアプリケーションの実行
* バッチアプリケーションの実行結果の確認

ここでは、 :doc:`../introduction/start-guide` で紹介したDirect I/O をを使ったサンプルアプリケーションをAsakusa Frameworkの標準設定のままで実行する例を示します。

動作確認用テストデータの配置
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

バッチアプリケーションの実行に必要な入力データを配置します。

以下は、Hadoopクライアントマシン上に配置した入力データファイルを :program:`hadoop` コマンドでHadoopファイルシステムに登録する例です。

..  code-block:: sh

    hadoop fs -put /path/to/example-dataset/master target/testing/directio/master
    hadoop fs -put /path/to/example-dataset/sales target/testing/directio/sales

..  hint::
    実行するバッチアプリケーションが利用する外部システム連携機能によって、入力データの配置箇所は異なります。
    例えば、WindGate/JDBC を利用する場合はデータベースに対して入力データを配置します。

..  hint::
    |M3BP_FEATURE| を標準の設定で利用する場合、Direct I/Oのファイルシステムパスは通常OS標準のファイルシステムに対応します。

    このため、入力データを配置する例は以下のようになります。

    ..  code-block:: sh

        mkdir -p "$HOME/target/testing/directio"
        cp -a /path/to/example-dataset/master $HOME/target/testing/directio/master
        cp -a /path/to/example-dataset/sales $HOME/target/testing/directio/sales

    なお、 |M3BP_FEATURE| をHadoopファイルシステムと連携して利用する方法については、 :doc:`../m3bp/user-guide` の「Hadoopとの連携」の項を参照してください。

バッチアプリケーションの実行
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

デプロイしたバッチアプリケーションをYAESSを使って実行します。

:program:`$ASAKUSA_HOME/yaess/bin/yaess-batch.sh` コマンドに実行するバッチIDとバッチ引数を指定してバッチを実行します。

:doc:`../mapreduce/index` を利用したバッチアプリケーションの場合、バッチIDをそのまま指定して実行します。

..  code-block:: sh

    $ASAKUSA_HOME/yaess/bin/yaess-batch.sh example.summarizeSales -A date=2011-04-01

:doc:`../spark/index` を利用したバッチアプリケーションの場合、バッチIDのプレフィックスに ``spark.`` を付与して実行します。

..  code-block:: sh

    $ASAKUSA_HOME/yaess/bin/yaess-batch.sh spark.example.summarizeSales -A date=2011-04-01

:doc:`../m3bp/index` を利用したバッチアプリケーションの場合、バッチIDのプレフィックスに ``m3bp.`` を付与して実行します。

..  code-block:: sh

    $ASAKUSA_HOME/yaess/bin/yaess-batch.sh m3bp.example.summarizeSales -A date=2011-04-01


バッチの実行が成功すると、コマンドの標準出力の最終行に ``Finished: SUCCESS`` と出力されます。

..  code-block:: none

    2013/04/22 13:50:35 INFO  [YS-CORE-I01999] Finishing batch "example.summarizeSales": batchId=example.summarizeSales, elapsed=12,712ms
    2013/04/22 13:50:35 INFO  [YS-BOOTSTRAP-I00999] Exiting YAESS: code=0, elapsed=12,798ms
    Finished: SUCCESS

バッチの実行が失敗した場合はYAESSのログを確認します。

バッチアプリケーションのシミュレーションモード実行
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

YAESSでは実際のアプリケーションの実行は行わず、環境構成や設定の確認のみを行うシミュレーションモード実行を行うことができます。

この機能は、バッチの失敗が環境構成や設定の問題であるか、アプリケーション内の問題であるかを切り分けるために有効です。

バッチをシミュレーションモードで実行するには、 :program:`yaess-batch.sh` コマンドの引数の末尾に ``-D dryRun`` と指定します。

..  code-block:: sh

    $ASAKUSA_HOME/yaess/bin/yaess-batch.sh example.summarizeSales -A date=2011-04-01 -D dryRun

バッチアプリケーションの実行結果の確認
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

バッチアプリケーションが出力したデータの内容を確認します。

Direct I/O をバッチの出力に利用するアプリケーションについては、以下のツールなどを利用してファイルシステム上のファイル内容を確認することができます。

* :program:`$ASAKUSA_HOME/directio/bin/list-file.sh <base-path> <resource-pattern>`

  * Direct I/Oの入出力ディレクトリやファイルの一覧を表示
* :program:`hadoop fs -text <file-path>`

  * Hadoopファイルシステム上の指定したファイルパスの内容を表示

関連するトピック
================

運用環境の構築や設定に関する情報として、以下のドキュメントも参考にしてください。

実行エンジンの最適化に関する設定
--------------------------------

以下のドキュメントでは、各実行環境のチューニングパラメータなどを説明しています。

* :doc:`configure-hadoop-parameters`
* :doc:`../spark/optimization`
* :doc:`../m3bp/optimization`

各コンポーネントの設定
----------------------

Asakusa Frameworkの各コンポーネントの設定に関しては、各コンポーネントのユーザーガイドなどを参照してください。

* :doc:`../directio/user-guide`
* :doc:`../windgate/user-guide`
* :doc:`../yaess/user-guide`

また、外部システム連携モジュールを用いた場合のシステム構成に関して以下のドキュメントで紹介しています。

* :doc:`deployment-architecture`

