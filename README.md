# cs-con-http-download

```cs

using System;
using System.IO;
using System.Net.Http;
using System.Threading.Tasks;

namespace cs_con_http_download
{
    class Program
    {
        // https://docs.microsoft.com/ja-jp/dotnet/csharp/tutorials/console-webapiclient
        static async Task Main(string[] args)
        {
            HttpClient client = new HttpClient();

            Byte[] data = await client.GetByteArrayAsync("https://winofsql.jp/fflogo.png");

            using (FileStream stream = File.Open("test.png", FileMode.Create))
            {
                using (BinaryWriter writer = new BinaryWriter(stream))
                {
                    writer.Write(data);
                }
            }
        }
    }
}
```

### [static async Task Main](https://docs.microsoft.com/ja-jp/dotnet/csharp/language-reference/proposals/csharp-7.1/async-main#code-try-3) : 非同期エントリポイント


```cs
using System;
// using System.Net;
// using System.Text;
using Newtonsoft.Json;

// using System.IO;
// using System.Net.Http;
// using System.Threading.Tasks;

// dotnet add package Newtonsoft.Json --version 13.0.1
// dotnet add package System.Text.Encoding.CodePages
namespace cs_con_web_json
{
    class Program
    {
        static async Task Main(string[] args)
        {
            string json_url = "https://lightbox.sakura.ne.jp/demo/template/basic/basic-html/project/basic-01.json";
            // WebClient webClient = new WebClient();
            // webClient.Encoding = Encoding.GetEncoding("utf-8");
            // string json_text = webClient.DownloadString(json_url);

            string result = "";

            HttpClient httpClient = new HttpClient();

            HttpResponseMessage? response = null;
            try {
                response = httpClient.GetAsync(json_url).Result;
            }
            catch (Exception Err) {
                result = "ERROR: " + Err.Message;
            }

            // try {
            //     response.EnsureSuccessStatusCode();
            // }
            // catch (Exception Err) {
            //     result = "ERROR: " + Err.Message;
            // }

            // 内容を文字列として取得
            try {
                String text = response.Content.ReadAsStringAsync().Result;

                result = text;
            }
            catch (Exception Err) {
                result = "ERROR: " + Err.Message;
            }

            Console.WriteLine(result);

            // JSON 文字列を一括でクラスのオブシェクトに変換
            MyJson data = JsonConvert.DeserializeObject<MyJson>(result);

            Console.WriteLine(data.title);
            Console.WriteLine(data.name);
            Console.WriteLine(data.image);
            Console.WriteLine(data.text);

            Console.ReadLine();

            Byte[] data2 = await httpClient.GetByteArrayAsync("https://img.furusato-tax.jp/img/x/gcf/project/forms/20200710/d_82f8d8b5cb154d4e2469a21f6402a956f0947cf6.jpg");

            using (FileStream stream = File.Open("test.jpg", FileMode.Create))
            {
                using (BinaryWriter writer = new BinaryWriter(stream))
                {
                    writer.Write(data2);
                }
            }

        }

        // ******************************************
        // 一括変換用のクラス
        // ******************************************
        private class MyJson
        {
            public string title { get; set; }
            public string name { get; set; }
            public string image { get; set; }
            public string text { get; set; }
        }
    }
}
```
