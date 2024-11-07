# AI_POEM_GENERATOR

# **How to Build an AI Poem Generator using Next.js and Facebook’s LLaMA70B-v2 Chat Model**

Are you ready to dive into the mesmerizing world of poetry with the help of AI? In this blog, we'll take you on a poetic journey, showing you how to build your very own AI poem generator using Next.js and Replicate's cutting-edge LLaMA70B-v2 Chat Model.

### **Introducing LLaMA70B-v2 Chat Model**

The LLaMA70B-v2 Chat Model is the latest open-source language model from Meta. It has been trained on a whopping 2 trillion tokens, making it a powerhouse for generating creative text. With a mind-blowing 70 billion parameters, it's one of the largest models released in open source by Meta.

When using this model, you'll want to give it a nudge in the right direction by providing a structured prompt. In our examples, we keep it simple, leaving a blank space after "Assistant:" to let the model fill in the rest of the poem.

## **Prerequisites**

To follow along with this tutorial, you should have the following prerequisites:

1. Node.js and npm (Node Package Manager) installed on your machine.
2. Basic knowledge of JavaScript and React.
3. An active Replicate account and an API token. If you don't have one, you can sign up for an account on the [Replicate website](https://replicate.com/facebookresearch/musicgen).

## **Setting Up the Next.js Project**

Let's start by setting up a new Next.js project. Open your terminal and execute the following commands:

```bash

npx create-next-app ai-poem-generator

✔ Would you like to use TypeScript with this project? … No
✔ Would you like to use ESLint with this project? … Yes
✔ Would you like to use Tailwind CSS with this project? … Yes
✔ Would you like to use `src/` directory with this project? … Yes
✔ Use App Router (recommended)? … No
✔ Would you like to customize the default import alias? … No
```

This will create a new Next.js project in a directory named **`ai-peom-generator`**. Next, open the project in your preferred code editor.

```bash
cd ai-poem-generator
```

## **Installing Dependencies**

Next, we need to install the required dependencies for our project. In the terminal, navigate to the project directory and run the following command:

```bash
npm install replicate
```

Now, create a new file called **`.env`** in the root of your project and add the following environment variables:

```sql
REPLICATE_API_TOKEN=<paste-your-token-here>
```

Retrieve your API token from your [Replicate account settings](https://replicate.com/account/api-tokens).

## **Creating the Backend**

Next, we will create a new file called **`poem-generator.js`** inside the **`src/pages/api`** directory. This file will contain the code to generate AI poem using the Replicate API.

Add the following code to **`peom-generator.js`**:

```jsx
import Replicate from "replicate";

export default async function handler(req, res) {
  const replicate = new Replicate({
    auth: process.env.REPLICATE_API_TOKEN,
  });

  try {
    const output = await replicate.run(
      "replicate/llama70b-v2-chat:e951f18578850b652510200860fc4ea62b3b16fac280f83ff32282f87bbd2e48",
      {
        input: {
          prompt: `
          User: Can you write a poem about ${req.body.prompt}\n
          Assistant: `,
        },
      }
    );
    res.status(200).json({ poem: output });
  } catch (error) {
    console.error("AI poem generation failed:", error);
    res.status(500).json({ error: "AI poem generation failed" });
  }
}
```

The code sets up a handler function to initialize the Replicate client using your API token. It then calls the **`run`** method to generate a poem using the LLaMA70B-v2 Chat Model, based on the provided prompt.

## **Creating the Frontend**

Now that the backend is set up, let's create the frontend interface using Next.js and Tailwind CSS.

1. Open the **`pages/index.js`** file in your project and replace the existing code with the following:

```jsx
import { useState } from "react";

export default function Home() {
  const [poem, setPoem] = useState("");
  const [prompt, setPrompt] = useState("");
  const [isLoading, setIsLoading] = useState(false);

  const generatePoem = async () => {
    setIsLoading(true);

    try {
      const response = await fetch("/api/poem-generator", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ prompt }),
      });

      const { poem } = await response.json();
      setPoem(poem);
    } catch (error) {
      console.error("Failed to generate poem:", error);
    }

    setIsLoading(false);
  };

  return (
    <div className="flex flex-col items-center justify-center h-screen">
      <h1 className="text-3xl font-bold mb-6">AI Poem Generator</h1>
      <textarea
        value={prompt}
        onChange={(e) => setPrompt(e.target.value)}
        placeholder="Start typing your poetic inspiration here..."
        className="px-4 py-2 border border-gray-300 rounded-lg mb-4 w-80 h-40"
      />
      <button
        onClick={generatePoem}
        className="px-4 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600"
        disabled={isLoading}
      >
        {isLoading ? "Generating..." : "Generate Poem"}
      </button>
      {isLoading && (
        <div className="mt-4">
          <div className="animate-spin rounded-full h-8 w-8 border-t-2 border-blue-500"></div>
        </div>
      )}
      {poem && (
        <div className="mt-4">
          <p>{poem}</p>
        </div>
      )}
    </div>
  );
}
```

This code includes the frontend interface to interact with our AI poem generator. The UI contains a text area where users can input their poetic inspiration. When the "Generate Poem" button is clicked, a request is sent to the **`/api/poem-generator`** endpoint, and the generated poem is displayed below.

Here are a few examples of the prompt:

1. **love, mountains, romance. write this in Oscar wilde style**
2. **forest, solitude, lonliness. Write in the style of Alexander Pope**

## Conclusion

Voila! We've crafted our very own AI poem generator using Next.js and Replicate's incredible **LLaMA70B-v2** Chat Model. It's like having a poetic genius at our fingertips, turning our prompts into mesmerizing verses!

With a sprinkle of React and a dash of Tailwind CSS, we've whipped up a delightful interface that lets us channel our creativity. The "Generate Poem" button sets the AI into motion, and while it's busy conjuring verses, we've got a cool spinner keeping us entertained.

Now, our poetic journey has no limits. We can explore the realms of imagination, share our masterpieces, and let the AI-infused poetry captivate hearts all around. From haikus to sonnets, the possibilities are endless!

So, grab your pen, or well, your keyboard, and let's dive into the enchanting world of AI-generated poetry. Unleash your creativity, let the words flow, and together with Next.js and Replicate's magic, we'll create poetic wonders that'll leave everyone in awe. Happy writing, fellow wordsmiths!
