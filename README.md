# design_pattern
プログラムの良い設計書というかサンプルの一種

## なぜ必要だと思ったか
これ面接でも聞かれるんだよね。つまり、技術力の一つの要素になる。これ聞かれるとPHPerの自分は大概答えられない
非常に良くない。
というわけで、理解するに越したことはない

## オブジェクト指向の３大要素
* カプセル化
* 継承
* ポリモフィズム
* 

### 継承
* 説明しなくてもわかると思うので省略します。
* 作る際に意識してることは、別のプログラムでそのスーパークラスに修正がなくても再利用できるように、実際使ってる側で密接に絡まないように作ること
* 

### ポリモフィズム
* このページがすごいわかりやすい
* http://www.nulab.co.jp/designPatterns/designPatterns1/designPatterns1-4.html
* Interfaceを使うパターンとabstructを使うパターンの２つがある
* 

## AbstructとInterfaceの使い分け
どちらも似たような効果があり、どちらを使うか迷う

### Abstructの使いどころ
共通的な実装があり、サブクラスで変更したい部分のみを抽出して抽象メソッドにした場合、抽象クラスを使う

### Interfaceの使いどころ
クラス間の共通の振る舞いを定義したい場合、インタフェースを使う

## シングルトンパターン
インスタンスを生成するのってメモリ使うので、それをしないための、仕組み
多分一番よく使われるのでは
```
リスト1　StaffListCache.java
/**
* CSVファイルからデータを読み取り、リストの作成を行い
* キャッシュ処理を行うシングルトンクラスです。
*/
public class StaffListCache {

	private static Log log =     LogFactory.getLog(StaffListCache.class);
	private static StaffListCache instance = new StaffListCache();
	private Map stafflistmap = new HashMap();
	private StaffContext context = new StaffContext(
				AnalysisStaffCompanyA.COMPANYCODE_A);

	private StaffListCache() {
	}

	public static StaffListCache getInstance() {

		// オブジェクトを生成するのは、初めの1回
		if (instance == null) {
			instance = new StaffListCache();
		}

		return instance;
	}

	public List getStaffList(String filename) {

	  // ①キャッシュから取り出す
	  List list = (List)stafflistmap.get(filename);
②
	if (list == null) {
		list = new LinkedList();
		FileReader freader;
		try {
			freader = new FileReader(filename + ".csv");
			BufferedReader breader =
					new BufferedReader(freader);

			String line;
			while((line = breader.readLine()) != null) {
				// Staffオブジェクト生成
				list.add(context.getStaff(line));
			}
			stafflistmap.put(filename, list); //キャッシュ
			freader.close();
			System.out.println("ファイルから取得");
		} catch (IOException e) {
			log.error(e.getMessage(), e);
		}

	  } else {
		System.out.println("ファイルから取得");

	  }
	  return list;
	}
}
```


# 参考
* http://www.nulab.co.jp/designPatterns/designPatterns1/designPatterns1-1.html
