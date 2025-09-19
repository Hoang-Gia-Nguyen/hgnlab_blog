+++
title = "Hai công cụ terminal hay ho bạn nên biết: jump và thefuck"
date = 2025-09-19
draft = false
description = "Khám phá hai công cụ terminal giúp tiết kiệm thời gian và sửa lỗi dòng lệnh hiệu quả: autojump và thefuck."
[taxonomies]
categories = ["Technology"]
+++

Nếu bạn là lập trình viên, sinh viên, hay đơn giản là người thường xuyên làm việc với terminal, chắc bạn cũng hiểu hiệu suất là yếu tố quan trọng. Tiết kiệm vài giây ở mỗi thao tác nhỏ sẽ cộng lại thành một khoảng thời gian đáng kể, nhưng quan trọng hơn là nó giúp bạn không bị ngắt quãng dòng suy nghĩ. Hôm nay, mình muốn chia sẻ hai công cụ dòng lệnh nhỏ mà có võ đã trở thành một phần không thể thiếu trong công việc hàng ngày của mình: `autojump` và `thefuck`.

## `autojump`: Dịch chuyển tức thời trong cây thư mục

Bạn có thấy mệt mỏi khi phải gõ đi gõ lại `cd ../../mot/duong/dan/rat/dai` không? Mình thì có đấy.

**Nó là gì?**
[`autojump`](https://github.com/wting/autojump) là một cách nhanh hơn để di chuyển giữa các thư mục. Nó sẽ học thói quen của bạn và cho phép bạn "nhảy" đến các thư mục hay dùng chỉ với một lệnh đơn giản.

**Cách hoạt động**
`autojump` lưu lại một danh sách các thư mục bạn thường truy cập. Nó sắp xếp chúng dựa trên "frecency"—một sự kết hợp giữa tần suất (frequency) và sự gần đây (recency). Vì vậy, các thư mục bạn dùng nhiều nhất và gần đây nhất sẽ có điểm cao hơn. Khi bạn muốn di chuyển, bạn chỉ cần gõ một phần tên của thư mục, và `autojump` sẽ đưa bạn đến kết quả phù hợp nhất.

**Ví dụ**
Giả sử bạn thường làm việc trong thư mục `<duong_dan_rat_dai>/Workspace/hgnlab_blog/content`. Sau khi `cd` vào đó vài lần, `autojump` sẽ ghi nhớ. Từ đó về sau, dù đang ở đâu, bạn chỉ cần gõ:

```bash
j blog
```

Và bạn sẽ được đưa thẳng đến `<duong_dan_rat_dai>/Workspace/hgnlab_blog/content`. Siêu tiết kiệm thời gian!

## `thefuck`: Nút "undo" đa năng của bạn

Ai cũng có lúc gõ nhầm. `git psuh`, `puthon main.py`, `apt-get isntall...` Thật bực mình khi phải gõ lại cả một dòng lệnh dài chỉ để sửa một lỗi nhỏ.

**Nó là gì?**
[`TheFuck`](https://github.com/nvbn/thefuck) là một ứng dụng tuyệt vời giúp sửa lỗi cho câu lệnh bạn vừa gõ sai.

**Cách hoạt động**
Khi bạn chạy một lệnh và nó báo lỗi, bạn chỉ cần gõ `fuck`. `TheFuck` sẽ phân tích lệnh trước đó, tìm lỗi dựa trên một bộ quy tắc, và đưa ra gợi ý sửa lỗi. Bạn chỉ cần đồng ý, và lệnh đúng sẽ được thực thi.

**Ví dụ**

> Gõ nhầm lệnh git
```bash
$ git psuh
git: 'psuh' is not a git command. See 'git --help'.

Did you mean this?
        push

$ fuck
git push [enter/↑/↓/ctrl+c]
```

> Quên `sudo`
```bash
$ apt-get install vim
E: Could not open lock file /var/lib/dpkg/lock-frontend - open (13: Permission denied)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), are you root?

$ fuck
sudo apt-get install vim [enter/↑/↓/ctrl+c]
```

Cảm giác như có phép thuật và nó luôn làm mình thấy vui.

---

Cả `autojump` và `thefuck` đều là những tiện ích nhỏ nhưng mang lại lợi ích lớn. Chúng giúp quy trình làm việc của bạn trôi chảy hơn, giảm bớt sự bực bội và tiết kiệm một lượng thời gian đáng kể. Hãy dùng thử xem nhé!
