UMPで使用する場合はこちらです。
https://github.com/NKC-GameCreatorClub/FujiyoshiHinaka_JBSaveLoadLibrary.git?path=Packages/JBSaveLoad

// ご使用方法
①初めにusing SaveLoadLib;と記述する。
②SaveLoad.Instanceからライブラリを利用できます。

例
using UnityEngine;
using SaveLoadLib; //①

public class Test
{
	CharacterParameter data = new CharacterParameter();
	string filePath = "C:/SaveDatas/PlayerParameter.bin";

	void Start()
	{
		SaveLoad.Instance.Save(data, filePath); //②のセーブ関数を利用
	}
}


// セーブしたい
①セーブしたいデータを持つSerializable属性を付与した
　任意の「class」及び「struct」を用意する。
②SaveLoad.Instance.Save(①で用意したデータ,セーブ先のファイルパス)
　※ファイル名は"任意名.bin"で用意してください。
　※存在しないディレクトリ名(フォルダ名)を使用した場合自動的にディレクトリが生成されます。
　※存在しないファイルを使用した場合も自動的にファイルが生成されます。
　※セーブ内容は自動的に暗号化が行われます。

例
public class Test
{
	CharacterParameter data = new CharacterParameter();
	string filePath = "C:/SaveDatas/PlayerParameter.bin";

	void Start()
	{
		SaveLoad.Instance.Save(data, filePath); //②
	}
}

[System.Serializable] //①
public struct CharacterParameter
{
	public float Speed;
	public int MaxHP;
}


// ロードしたい
①ロードしたいデータと同じ型(「class」及び「struct」)を持った変数を用意する。
②SaveLoad.Instance.Load(out ロードしたデータを受け取る①, セーブデータの入ったファイルのパス)
　※戻り値はロードの結果です(true=成功false=失敗)。
　※ロードの成功失敗にかかわらず使用前の①のデータは書き換えられます。

例
public class Test
{
	CharacterParameter data; //①
	string filePath = "C:/SaveDatas/PlayerParameter.bin";

	void Start()
	{
		if(SaveLoad.Instance.Load(out data, filePath)/*②*/)
		{
			Debug.Log("成功！");
			return;
		}
		Debug.Log("失敗……");
	}
}


// 注意事項
・バイナリファイル(.bin拡張子のファイル)以外でのセーブについては動作保証できません。
・暗号化は排他的論理和(XOR)を利用したものであり
　復号(元データに戻す行為)を完全に防ぐことは保証できません。
・Unityエディター(エディターバージョン2022.3.20f1)以外での動作保証はできません。
・同封されているサンプルシーン(SaveLoadLibSample)でセーブを行うと
　Assetsを親フォルダとするSaveDatasフォルダが生成されます。
・本ライブラリを利用したファイル操作により起こる問題についての責任は負いかねますので
　自己責任でお使いください。
・パス指定におけるフォルダやファイルごとの区切りを示す記号は"\"または"/"をお使いください。
