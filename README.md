# seatChange
package seatChange2;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.time.LocalDateTime;

public class CodeBreakerFirst {
	public static void main(String[] args) {
		// TODO 自動生成されたメソッド・スタブ
		String title = "*** 数当てゲーム ***";// タイトル
        String rule = "隠された3つの数字をあてます。\n"
                    + "1つの数字は1から6の間です。\n"
                    + "3つの答えの中に同じ数字はありません。\n"
                    + "入力した数字の、"
                    + "位置と数字が当たってたらヒット、\n"
                    + "数字だけあってたらボールになります。\n"
                    + "全部当てたら(3つともヒットになったら)"
                    + "終了です\n\n";
        
        String pointDescription = "[ポイント説明]";
        String countPoint = "1から3回目で10ポイント\n"
        					+ "4から6回目で8ポイント\n" 
        					+ "7から10回目で5ポイント\n"
        					+ "11から13回目で3ポイント\n"
        					+ "13回以上で0ポイント\n";
        String timePoint = "30秒で10ポイント\n"
        					+ "1分で8ポイント\n"
        					+ "1分30秒で6ポイント\n"
        					+ "2分で4ポイント\n"
        					+ "2分30秒で2ポイント\n"
        					+ "3分以上で0ポイント\n";

        int[] answer = new int[3];// 答えが入る
        int[] input = new int[3];// 入力した答えが入る
        /*
         * hitはhitのカウント用、blowもblowのカウント用。
         * countは何回目のチャレンジかを数えている。
         */
        int hit = 0, blow = 0, count = 0;
      //処理開始前の時刻
    	//long start_point = System.currentTimeMillis(); 
        LocalDateTime start = LocalDateTime.now();
        //タイトルとルールの表示
        BufferedReader br
            = new BufferedReader(new InputStreamReader(System.in));
        System.out.println(title);
        System.out.println(rule);
        System.out.println(pointDescription);
        System.out.println(countPoint);
        System.out.println(timePoint);
      
      //ランダムな答えを作成。
        //ただし、仕様通り、同じ数字がないようにする。
        for (int i = 0; i < answer.length; i++) {
            //自分より前の要素にかぶるやつがないか確かめる。
            //あったらもう1回random
            boolean flag = false;
            answer[i] = (int) (Math.random() * 6 + 1);
            do {
                flag = false;
                for (int j = i - 1; j >= 0; j--) {
                    if (answer[i] == answer[j]) {
                        flag = true;
                        answer[i] = (int) (Math.random() * 6 + 1);
                    }
                }

            } while (flag == true);
        }
        //ゲーム部
        while (true) {
            count++;
            System.out.println("*** "+count + "回目 ***");
            //インプット
            for (int i = 0; i < answer.length; i++) {
                System.out.print( (i + 1) + "個目 : ");
                try {
                    input[i] = Integer.parseInt(br.readLine());
                } catch (NumberFormatException e) {
                    System.err.println("数値を入力してください");
                    i--;
                } catch (IOException e) {
                    System.err.println("もう一度入力してください");
                    i--;
                }
            }
            //答え判断
            hit = 0;
            blow = 0;
            for (int i = 0; i < answer.length; i++) {
                for (int j = 0; j < answer.length; j++) {
                    if (i == j && input[i] == answer[j]) {
                        hit++;
                    } else if (input[i] == answer[j]) {
                        blow++;
                    }
                }
            }
            //終了判断　ヒットが3つになったら終了
            System.out.println("ヒット" + hit + " ボール" + blow);
            if (hit == 3) {
                System.out.println("おめでとー");
                break;
            }else{
                System.out.println();
            }
        }
        //処理終了時の時刻
        LocalDateTime end = LocalDateTime.now();
    	//ゲームにかかった時間
        int gameMinute = end.getMinute() - start.getMinute();
        int gameSecond = end.getSecond() - start.getSecond();
        System.out.println("ゲームにかかった時間は" + gameMinute + "分" + gameSecond + "秒です");
	}
}
