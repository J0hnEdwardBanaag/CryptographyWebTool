To create an HTML and JavaScript program that allows you to upload Markdown or text files, accept a secret key, and then encrypt the content, you can use the following example. This example uses the CryptoJS library for encryption and the FileReader API for file uploads. Please note that this is a basic example for educational purposes and should not be used for real security-sensitive applications without further improvements.

Here's the HTML and JavaScript code:

```html
<!DOCTYPE html>
<html>
<head>
  <title>File Encryption</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/3.1.9-1/crypto-js.js"></script>
</head>
<body>
  <h1>File Encryption</h1>
  
  <input type="file" id="fileInput" accept=".md, .txt">
  <br><br>
  <input type="text" id="secretKey" placeholder="Enter Secret Key">
  <br><br>
  <button onclick="encryptFile()">Encrypt</button>
  
  <div id="output">
    <h2>Encrypted Content</h2>
    <textarea id="encryptedContent" rows="10" cols="40"></textarea>
  </div>

  <script>
    function encryptFile() {
      const fileInput = document.getElementById("fileInput");
      const secretKeyInput = document.getElementById("secretKey");
      const outputTextarea = document.getElementById("encryptedContent");

      const file = fileInput.files[0];
      const secretKey = secretKeyInput.value;

      if (!file) {
        alert("Please select a file to encrypt.");
        return;
      }

      if (!secretKey) {
        alert("Please enter a secret key.");
        return;
      }

      const reader = new FileReader();
      reader.onload = function(e) {
        const fileContent = e.target.result;
        const encryptedContent = CryptoJS.AES.encrypt(fileContent, secretKey).toString();
        outputTextarea.value = encryptedContent;
      };

      reader.readAsText(file);
    }
  </script>
</body>
</html>
```

In this code:

1. We create an HTML form that allows the user to select a file and enter a secret key.
2. We use the CryptoJS library for AES encryption.
3. When the "Encrypt" button is clicked, it reads the selected file's content and encrypts it using the secret key.
4. The encrypted content is displayed in a textarea.

Please note that this example uses client-side encryption, and the security of the encryption relies on the secrecy and strength of the secret key. For real-world use cases, you should consider stronger encryption methods and more secure practices.





To create a program that allows you to upload an encrypted text file, accept a secret key, decrypt the content, and provide an option to show/hide the decrypted content and download it, you can modify the previous HTML and JavaScript code. Here's an updated example:

```html
<!DOCTYPE html>
<html>
<head>
  <title>File Encryption and Decryption</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/3.1.9-1/crypto-js.js"></script>
</head>
<body>
  <h1>File Encryption and Decryption</h1>

  <input type="file" id="fileInput" accept=".enc">
  <br><br>
  <input type="text" id="secretKey" placeholder="Enter Secret Key">
  <br><br>
  <button onclick="decryptFile()">Decrypt</button>
  
  <div id="decryptedContent" style="display: none;">
    <h2>Decrypted Content</h2>
    <button onclick="toggleDecryption()">Show/Hide</button>
    <br><br>
    <textarea id="decryptedText" rows="10" cols="40"></textarea>
    <br>
    <a id="downloadLink" style="display: none;" download="decrypted.txt"><button onclick="downloadDecryptedFile()">Download Decrypted File</button></a>
  </div>

  <script>
    function decryptFile() {
      const fileInput = document.getElementById("fileInput");
      const secretKeyInput = document.getElementById("secretKey");
      const decryptedContentDiv = document.getElementById("decryptedContent");
      const decryptedText = document.getElementById("decryptedText");
      const downloadLink = document.getElementById("downloadLink");

      const file = fileInput.files[0];
      const secretKey = secretKeyInput.value;

      if (!file) {
        alert("Please select an encrypted file to decrypt.");
        return;
      }

      if (!secretKey) {
        alert("Please enter the secret key.");
        return;
      }

      const reader = new FileReader();
      reader.onload = function(e) {
        const encryptedContent = e.target.result;
        try {
          const decryptedContent = CryptoJS.AES.decrypt(encryptedContent, secretKey).toString(CryptoJS.enc.Utf8);
          if (decryptedContent) {
            decryptedText.value = decryptedContent;
            decryptedContentDiv.style.display = "block";
            downloadLink.style.display = "block";
          } else {
            alert("Decryption failed. Please check the secret key.");
          }
        } catch (error) {
          alert("Decryption error: " + error.message);
        }
      };

      reader.readAsText(file);
    }

    function toggleDecryption() {
      const decryptedText = document.getElementById("decryptedText");
      const downloadLink = document.getElementById("downloadLink");

      if (decryptedText.style.display === "none") {
        decryptedText.style.display = "block";
        downloadLink.style.display = "block";
      } else {
        decryptedText.style.display = "none";
        downloadLink.style.display = "none";
      }
    }

    function downloadDecryptedFile() {
      const decryptedText = document.getElementById("decryptedText").value;
      const downloadLink = document.getElementById("downloadLink");
      const blob = new Blob([decryptedText], { type: "text/plain" });
      downloadLink.href = URL.createObjectURL(blob);
    }
  </script>
</body>
</html>
```

In this updated code:

1. The user can select an encrypted text file (with the .enc extension).
2. After entering the secret key and clicking "Decrypt," the program attempts to decrypt the file's content.
3. The decrypted content can be shown or hidden using the "Show/Hide" button.
4. There's a "Download Decrypted File" button that allows the user to download the decrypted content as a text file.