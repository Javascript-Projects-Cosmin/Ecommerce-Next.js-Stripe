This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

Notes:
- Created using Next.js using npx create-next-app
- npm install --legacy-peer-deps
- npm run dev to run the project in development mode
- npm install -g @sanity/cli
- sanity init --coupon javascriptmastery2022
- sanity manage to control the CNS
- sanity start will start the manager or localhost:3333

We started by creating a product schema in sanity, similar to a noSql database. Then we created another schema for a banner. Don't forget when you modify the schemas, you also change schema.js to import them right

For our next.js part, we first added the babel depend. in eslint and the .babelrc file. (Note, lots of trouble with babelrc compile, fixed it by adding a the babel modules)

After adding our global styles, we're going to create components. We start with the hero banner by statically for now, but writing this because another error was encountered: babel regenerateruntime failed, we had to add 2 new deps for it to work...

We will hook us Sanity right away by creating in the lib directory a client.js file, which will contain our client connection (Remember the token is grabbed from the sanity manager API)

In case of Next.js, unlike React where we would use the UseEffect lifecycle method, we have tto use:
export const getServerSideProps = async () => {
    const query = '*[_type == "product"]'; //grab all products from sanity
    const products = await client.fetch(query);

    const bannerQuery = '*[_type == "banner"]'; 
    const bannerData = await client.fetch(bannerQuery);

    return {
        props: {products, bannerData}
    };
}
This is a function to query data and load it into pages, and we pass the data back to the Home component, and now we can customize the hero banner with data from our Sanity.
We pass this data to the Herobanner and Product components and dynamically render our landing page.

After getting our products and banner done, we dealt with the Layout component. An encompass of the others which we included in the app.js and it contains the Navbar and the footer.

The next big part of the project is the Product Details pages. In the pages directory, we'll create a product directory with a [slug].js inside it. This time, we'll use something simillar to the index, by grabbing a specific product from our specific path, using next.js with getStaticProps. We also created GetStaticPaths which gets product data, but only what is the current slug, that means title, price and image for the you may also like scenario.

And for the part we've been waiting for, the state part. In this case we are going to use it in the StateContext file, keeping a general state for all the application. And then we wrap the entire Layout with the context

We will use the context to function properly the cart item, and that is the static site mostly done. One last item on our list is the Stripe functionality which is the only item i am new at.
In case of Next.js we don't need a Express server, we can do everything in this API folder we have.

Stripe is a neat payment tool, so the only thing left is the success page in the components