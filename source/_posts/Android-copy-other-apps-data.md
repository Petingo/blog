---
title: Android 複製其他 APP 在 /data/data/ 中的資料
date: 2016-02-28 00:51:50
tags:
- Android
- 資工
---

### 緣起
最初會用到這玩意兒，是高中科展時為了要撈出 Line、Facebook messenger 的聊天訊息進行分析。當時為了這東西不知道搞了多久，最後還是靠隊友 Carry 才生出這份 code。

後來做的神魔關卡備份外掛也是靠這它才得以順利完成，再謝謝一次神奇的小夥伴。

### 為什麼
Android APP 在執行時會被限制在一個沙盒（sandbox）中，對於系統資源的存取會被隔離，大致如下圖所示；因此若要存取別的 APP 存放在 /data/data/ 中的資料，勢必得從其他地方下手。

<div class="mxgraph" style="width:100%;border:1px solid transparent;margin:0 auto;" data-mxgraph="{&quot;highlight&quot;:&quot;#0000ff&quot;,&quot;nav&quot;:true,&quot;resize&quot;:true,&quot;toolbar&quot;:&quot;zoom layers lightbox&quot;,&quot;edit&quot;:&quot;_blank&quot;,&quot;xml&quot;:&quot;&lt;mxfile userAgent=\&quot;Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.139 Safari/537.36\&quot; version=\&quot;8.6.8\&quot; editor=\&quot;www.draw.io\&quot; type=\&quot;google\&quot;&gt;&lt;diagram id=\&quot;df69082b-bde0-2ea9-3d78-96e4c49bb533\&quot; name=\&quot;Page-1\&quot;&gt;7Vttb6s2FP41kbZJtwKDDfnY9rbbtDvpStW0u0+TAw5hJTgDp0n262deHMA4gSRAuE1TqYFjY8x5HtvnOTgT43G5/TnCq8Xv1CXBBGjudmJ8ngBgTTX+PzHsMgMEembwIt/NTCXDi/8fyY35dd7ad0lcqcgoDZi/qhodGobEYRUbjiK6qVab06B61xX2SM3w4uCgbv3Td9kis9rAKuy/EN9biDvraJqVzLDz6kV0Heb3mwBjnn6y4iUWbeUPGi+wSzclk/E0MR4jSll2tNw+kiBxrXBbdt3zgdJ9vyMSsjYXQJP32dJ17nnLcmbuJ5C18IaDNRGPkHaU7YRz0scjSQPaxHjYLHxGXlbYSUo3nA3ctmDLgJ/p/HDuB8EjDWiUXmu4mNhzh9tjFtFXUipBjk1mc16Sd4BEjGwPPpW+9xWnIKFLwqIdr5JfAIzcvTn99rzaFGDqMLctykAKI84J5O3bLpzID3I/tvSp0bFPu/CQKXkIKDxkqDyk9eEh2OwhErr3ycjmZyENSdUjZOuzb7nzkuO/kuM7mJyFvG/fRLXkpCjL7kHc2lwgeZH3g64jhxyHl+HII+xYnToaJW+r6ChsEQkw89+q3VQBkN/hK/X5AxwcDqYpYZg9Xn5VebaQGjJk1iBYbSjzQa2hlA/7xz6PInadERy4l/yURmxBPRri4KmwPlRHlYovGUdyxiQl/xDGdvmChNeMclPR9hdKV5Ux2MydOuaXgjmVMJCnrLZgNjbUI5hiPq4MeBSwZMGgaScLnNG/ayoKPsUpMve8gm6vtkUhP/KSb4cu7/AdXq100d4sEmXCwnub3USYJV7xWZVV6VJdrPLpp7yy5SYc+F7ITx3OAcLtD8kc7fOw4j4vWPqumzJTNcNX2drFJA9h8ySvHZl3Op3jdVVo0QXmn3Hw5r9+4F3H27oq3vWw52tEHRLHdah+4PX++JVX0fhH/1GF5bsFDVVBMxSD1FZgJq/h3WBmNgdiF4X/kNiuqQr/bTAzEOrGpaY2qvBfbxHdDh3/m/qo4n8dNbtoOAFwNNoXaB4N90WlFoAMIwHkIXG2BIAycQaUALpVJ8VNagATdaQBGhvqE01bMea70gCzRAOAj5gQmGBUGmDaE+YfGuAQ3lfVAIJsp2oAXbspDWAaI9IAovt9aYC57RBH+QpgZkMTduRSaEvj4LoaQPhwTBoATlssDcNpANDiNclINIDo2VENICqNRgPIQ+JsDYBk4gyoAYBZJ8VNagBkSiCcqwEaG+oTTVVqpBwPVqO41jEiUsWI8S5mZPl3TCI+bR4MDG8+gkRtUkPDRZCqzNCHYugT7+sqBquGdzvFcFtvDRCognZdxaDK5JwSuWUgiJ1W4KJldaAtFVI+1bDPXH0tq6GhPldfVTamS6UHkz+V0kPpp5uhAOShMK0PBaiSMVYfY0HcqOTTL3645guQ9huJeOz4fuckUwok91mNhjlJh33goMpiPLuY4eLr+GaRE2FK3m6iGYJIMQ7mc5BmPEYGWG1fWNscCehl5KhSJArEDqf23z9itdd410VMlbF55jKL///pFuBAGmyEYzg0VHs3nl3ydjNooDGhcdqWZifAcexnWXEcsbr5aLS8z2+W0p0ikaVOcRUuv3z7s/iBxtFkqBq56wTr+z0MJ2+ZhdL0i4YL1o3Ttsh8z4QCLQiV7VQbDaPkOeRsRsmipE9G1VMt75VRbX6icd2Egm5VF6/zGSVtABiUUaclgr5nRpkfjLqcUfy0+KljVr34Oanx9D8=&lt;/diagram&gt;&lt;/mxfile&gt;&quot;}"></div>
<script type="text/javascript" src="https://www.draw.io/js/viewer.min.js"></script>

<!--more-->

### 所以要怎麼做？
~~複製貼上就可以了。~~
基本上就是先拿 Root 權限，然後執行 linux 的複製指令
{% codeblock lang:java %}
public static void copy(string resorsePath, string targetPath){
    Process suProcess = null;
    try {
        suProcess = Runtime.getRuntime().exec("su");
        DataOutputStream os = new DataOutputStream(suProcess.getOutputStream());
        os.writeBytes("mkdir -p " + targetPath + "\n");
        os.writeBytes("cp -r " + resorsePath + " " + targetPath + "\n");
        os.writeBytes("exit\n");
        os.flush();

    } catch (IOException e) {
        e.printStackTrace();
    }
}
{% endcodeblock %}

收工！