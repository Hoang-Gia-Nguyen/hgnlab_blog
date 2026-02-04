+++
title = "The 10MB Clipboard Share App: Why I Built My Own Text-Sharing Tool"
date = 2026-02-04
description = "A reminder that sometimes the best solution to a self-hosting headache is a simple script, a Docker container, and a plain .txt file."
authors = ["hgn"]
[taxonomies]
categories = ["Self-hosting"]
tags = ["self-hosting", "hobby"]

[extra]
cover.image = "images/clipboard-share-cover.png"
cover.alt = "Clipboard Share"

+++
# Introduction
How hard can it be to copy a string of text from one computer to another? If you’re self-hosting, the answer is often "surprisingly difficult. I’ve cycled through the heavy hitters—Ghostboard, Etherpad, and PrivateBin—searching for a seamless way to sync my clipboard. But between nagging WebSocket errors, bloated RAM usage, and security configurations that felt like overkill, the "solutions" were becoming more work than the problem. I didn't need a suite of features; I just needed a way to move text. So, I decided to build it myself.

# The Need for a Simple Solution
Given these challenges, I sought a lightweight, easy-to-deploy service that could share text across machines without requiring complex configurations or consuming too many resources.
- **Minimal Footprint**: If it uses more than 20MB of RAM, it's too heavy.
- **Persistent but Simple**: No complex database (PostgreSQL/Redis) required for just a few strings of text.
- **Universal Access**: Needs to work via curl in the terminal and a simple web UI for mobile. Need to have single url without any complex path.

I realized I didn't need a full-blown collaborative editor. I need something where I could 'POST' a snippet from my server and 'GET' it from my laptop instantly.

# Implementation & Deployment
The PHP script is straightforward:
```php
<?php
$file = 'data.txt';

// POST
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $input = $_POST['text'] ?? '';
    if (file_put_contents($file, $input) !== false) {
        echo "OK";
    } else {
        header('HTTP/1.1 500 Internal Error');
        echo "Lỗi: Không có quyền ghi vào file data.txt";
    }
    exit;
}

// GET
$content = file_exists($file) ? htmlspecialchars(file_get_contents($file)) : '';
?>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Quick Note</title>
    <style>
        body { margin: 0; background: #eee; font-family: sans-serif; }
        #status { position: fixed; top: 5px; right: 5px; font-size: 12px; color: gray; }
        textarea { width: 100vw; height: 100vh; padding: 20px; border: none; outline: none; font-size: 18px; box-sizing: border-box; }
    </style>
</head>
<body>
    <div id="status">Sẵn sàng</div>
    <textarea id="note"><?php echo $content; ?></textarea>

    <script>
        const note = document.getElementById('note');
        const status = document.getElementById('status');
        let timer;

        note.addEventListener('input', () => {
            status.innerText = "Typing...";
            clearTimeout(timer);
            timer = setTimeout(async () => {
                status.innerText = "Saving...";
                const fd = new FormData();
                fd.append('text', note.value);

                try {
                    const res = await fetch('', { method: 'POST', body: fd });
                    if (res.ok) {
                        status.innerText = "Saved at " + new Date().toLocaleTimeString();
                    } else {
                        status.innerText = "SERVER ERROR (Check write permissions)";
                    }
                } catch (e) {
                    status.innerText = "CONNECTION ERROR";
                }
            }, 800);
        });
    </script>
</body>
</html>
```

To make deployment easy and efficient, I packaged this script into a Docker image. This allows me to run the service on any machine that supports Docker. The base is the smallest, most simple php server image.

```dockerfile
FROM php:8.1-apache-bullseye

WORKDIR /var/www/html

COPY ./app/index.php /var/www/html/index.php
COPY ./app/data.txt /var/www/html/data.txt

RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html

EXPOSE 80

CMD ["apache2-foreground"]
```

When running, user only need to mount their own txt file to /var/www/html/data.txt.

**Security Warning**: Since this may handle sensitive text like API keys, I keep it locked behind my Cloudflare Access, ensuring only my devices can reach the endpoint. I also delete the sensitive text right after used. Maybe in near future I will add feature to auto clear note every few hours.

# Efficiency: Living on the Edge (of a VM)
I didn't want a clipboard manager that required its own dedicated server resources. By stripping away the bloat, I reduced the memory overhead to a mere **10MB**.

This efficiency allowed me to tuck the service away on a tiny ***Google Cloud e2-micro VM (1GB RAM)***.

Beyond the low memory usage, I opted for a flat-file storage system. Instead of managing a database engine like SQLite or PostgreSQL, the service reads from and writes to a single .txt file. This means backups are just a file copy away, and there’s zero overhead for database connections or migrations. It’s as "plug-and-play" as self-hosting gets.

# Conclusion: The Power of "Good Enough"
Building this was a reminder of why I started self-hosting in the first place: the freedom to solve my own problems with tools tailored exactly to my needs.

- **The Joy of Custom Gear**: There is a unique satisfaction in using a tool you built from scratch. It doesn't just solve a problem; it fits my workflow like a glove.
- **Awareness of the Trade-offs**: I’m the first to admit this isn't a "production-ready" enterprise solution. It lacks the bells and whistles of a commercial product, and I'm aware of the architectural flaws—but in a home lab, simplicity is a feature, not a bug.
- **A Foundation for Growth**: This is version 1.0. Knowing the limitations gives me a roadmap for what to learn next, whether that’s adding encryption, improving concurrency, or refining the UI.

Sometimes, we spend more time configuring complex tools than actually using them. By building a 10MB service that runs on a text file, I reclaimed my time and my system resources. It isn’t perfect, but it solves my problem perfectly—and for now, that’s exactly what I needed.

# GitHub Repository
You can find the code for this project on my [GitHub page](https://github.com/Hoang-Gia-Nguyen/simple-clipboard-share).