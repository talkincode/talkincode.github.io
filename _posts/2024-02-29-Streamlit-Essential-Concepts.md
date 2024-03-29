---
layout: post
title:  "Streamlit 程序运行必须知道的重要概念"
date:   2024-02-29 21:26:14 +0800
categories: streamlit
---

## 1. Streamlit 应用程序是自上而下运行的 Python 脚本

Streamlit 应用程序本质上是一个自顶向下执行的 Python 脚本。这意味着，当你启动应用程序时，Streamlit 引擎会从脚本的第一行开始执行代码，一直运行到最后一行，就像你在 Python 解释器中运行任何标准脚本一样。这种执行方式简化了流程，使得编写交互式Web应用更加直观。它允许开发人员用一个线性的、顺序的方式来思考他们的代码，同时提供了动态更新用户界面（UI）的能力，以响应用户的输入和其他触发事件。

## 2. 每次用户打开指向你的应用程序的浏览器标签页时，脚本就会被执行，并开始一个新的会话

每当有人在浏览器中打开你的 Streamlit 应用程序的新标签页，Streamlit 服务器就会响应这个新的连接请求，在服务器端启动一个新的脚本执行过程，这个过程被称为一个"会话"（session）。在一个会话中，用户的每个交互都有可能触发脚本的重新执行，并且每个会话是独立的，拥有自己的状态和变量。简而言之，新会话的开始意味着你的 Streamlit 应用被重新运行，创建一个为当前用户量身定制的交互式体验。

## 3. 脚本执行时，Streamlit 会在浏览器中实时绘制输出

脚本在执行过程中，Streamlit 会把每个操作的结果实时地显示在用户的浏览器上。这包括创建的图表、数据表格、文本输出等。一旦 Streamlit 命令被执行，比如 `st.write` 或 `st.pyplot`，它们所产生的内容就会立即出现在Web页面上。这个特性让开发者能够快速地创建和预览他们的应用程序界面，同时也确保了最终用户可以及时看到他们的操作如何影响输出结果。

## 4. 每当用户与窗口小部件交互时，脚本就会重新执行，Streamlit 会在浏览器中重新绘制输出， 该部件的输出值与重新运行时的新值相匹配

当用户与Streamlit应用程序中的控件，如滑块、按钮或输入框等互动时，整个应用程序脚本通常会从头开始重新执行。这个机制允许应用程序响应交互，根据用户的输入更新UI。例如，当用户移动一个滑块时，脚本会使用这个新的滑块值重新运行，可能会生成一个基于该值的新图表。然后，这个新图表会替代旧的图表显示在浏览器中。

这种处理方式是Streamlit的核心特性之一，它确保了应用的状态总是最新的，反映了用户最近的交互。此外，对于开发者来说，这使得编写应用逻辑变得简单，因为他们只需要设定好当控件值改变时应如何更新应用的状态，而不需要编写复杂的事件处理代码。

由于每次交互都会触发脚本的完整执行，Streamlit优化了这个过程，使其速度很快，并通过智能缓存减少不必要的计算，从而确保用户体验流畅。因此，当你与控件交互时，可以预期到一个迅速而流畅的响应，而且输出的结果始终与当前控件的状态相匹配。

## 5. 脚本使用 Streamlit 缓存来避免重新计算昂贵的函数，因此更新速度非常快

Streamlit的缓存机制是其性能优化的重要特点。使用`@st.cache`装饰器，开发者可以指示Streamlit对某个函数的输出进行缓存。这意味着，当函数的输入参数没有变化时，如果再次调用该函数，Streamlit将不会重新执行函数，而是直接使用缓存中的结果。这大大减少了那些计算成本高、响应时间长的函数的调用次数，从而保证了应用程序的快速响应和更新。通过这种方式，即便是数据处理或模型预测等重量级操作，在首次执行和缓存后，随后的调用就能迅速呈现，无需用户等待重复的计算过程。这样极大地提升了用户的交互体验和应用程序的性能。

## 6. 会话状态（Session State）可让你在重运行时保存持续存在的信息，而你需要的不仅仅是一个简单的小工具

Streamlit提供了一种名为会话状态（Session State）的机制，允许开发者在应用的多次运行之间保存数据。会话状态可被视作一个持久的存储空间，它能在不同的执行周期内保持信息，从而保存跨多个脚本重运行的用户交互状态。

借助会话状态，可以跟踪诸如用户输入、选择的结果、或者其他变量的状态等，而无需重新计算这些值。这使得应用程序在用户与应用交互时，除了重新计算小部件（例如按钮、文本输入等）以响应用户操作之外，还能保留之前的交互结果。因此，开发者可以构建更复杂的用户交互模式，例如多步骤表单或交互式数据探索，而不是仅仅依赖每次都从头到尾重新运行整个应用程序的简单小工具。

使用Streamlit的会话状态特性，即便是在Web应用中实现复杂的状态逻辑也变得简单和直观。这样不仅增强了用户体验，而且为开发者管理内部状态提供了方便。

## 7. Streamlit 应用程序可以包含多个页面，这些页面分别定义在 pages 文件夹下的 .py 文件中

Streamlit的多页面应用程序允许开发者在单独的Python脚本中创建不同的页面，且这些脚本放置于专门的 `pages` 文件夹内。每个 `.py` 文件代表了应用中的一个页面，Streamlit会自动为每个脚本生成导航菜单项。这样的组织方式方便了页面的管理和代码结构的维护，同时为用户提供了平滑的导航体验。通过简单的页面导航，用户可以轻松切换到不同的视图和功能，而开发者可以专注于单个页面的独立开发，无需担心不同页面间的代码干扰。
