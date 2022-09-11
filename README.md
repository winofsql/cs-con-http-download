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
