# File Based Routing 

 - React uses additional libraries like router-dom and react-router- dom to allow 'routing' in SPAs. 
 - Next integrates these libraries within its framework so they do not need to be installed separately.
 - In Next you create React component files and let Next.js infer the routes from the folder structure

![screenshot-gluki udemy com-2023 07 31-11_57_09](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/393a9c3e-1173-4a05-b7e7-19d3d2077944)

##  In a route "domain.com/somehting/[id] - how do you get access to id?

### Client Side
 - you can use the Next useRouter hook which is imported from the next/router package - this is a React hook defined by the Next.js team that can be used in any functional component (you can us withRouter HOC if working with class components)
 - the useRouter hook provides various data and methods: 

 - *pathname* - provides you with the URL **inferred by Next.js** from the folder structure. For example "/something/[id].
 -  *query* - returns an object containing a key for any concrete data encoded in the URL. For example: 
	 - path like `something/[id]` encoded as `something/1` would return `{id: "1"}`
	 - path like `something/[id]/[clientProjectId]` with URL of  `something/1/5` would return `{id: "1", clientProjectId: 5}`.

![screenshot-gluki udemy com-2023 07 31-12_35_30](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/d3e40423-4ee0-4071-a8ad-b4f8744c43a3)

On the client side, it would also make sense to handle what happens if the data that is being fetched is either loading or unavailable. Can use an if statement for that. 

### Server Side

 - you can use the context prop available in getStaticProps and getServerSideProps to gain access to the value.
 - for example,  if there is a [pid] file, the way to access the pid value would be as such:
   
<img width="1413" alt="Screenshot 2023-08-03 at 10 13 22" src="https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/6cb72101-9a52-4aa4-b568-2775509b3f72">

### Catch all Routes

<img src="https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/537b7b45-27bf-429e-818e-a5ffa552b6f1" width="300"/>

 - using the spread operator and the value name of your choice, you can catch all URL fragments that in this case, would follow `domain.com/blog`.
 - For example, using `useRouter` and `query` property on `domain.com/blog/2022/09` would return an object like this: `{slug: ["2022", "12"]}`.
 - the catch all routes file will only be executed if the number of dynamic values in the URL is sufficient for its execution. So for example, if the [...slug].js file sits on the same level as [id].js file in 'something' folder, then any URL with domain.com/somehting/1246 will trigger the latter. Only URLs with more than one dynamic value following soehting will trigger the former. For example, domain.com/1233/12334.  

## Navigation

 - You need to use the Link component from next/link which takes href as an attribute. 
 - Link has advantage over the anchor tag in that it does not send out a new http request everytime the link is clicked and therefore the state of the application is preserved. 
 - to generate dynamic links, you can simply fetch all the data on which the links will be based and map over the same to create link for each peace of data.

![screenshot-gluki udemy com-2023 07 31-14_07_44](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/55722112-9994-43ed-ae6c-f542bcfa7878)


### Navigating programatically i.e. when on want to navigate to a page upon certain event happening such as a form being submitted for example

 - Link would not work here as clicking the link does not allow you to perform additional actions such as submitting a form. Link is used to simply navigate to a new page and does nothing else.
 - in order to navigate to a new page programatically upon certain event happening, you can use the useRouter hook's push method. You can declare the hook at the top level of the component and then use its return value inside of a handleClickFunction which is attached to a submit button: 

![screenshot-gluki udemy com-2023 07 31-14_21_00](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/2b040358-49e7-4089-b227-94997b2bf126)

## Custom 404 Page

Just add a 404.js / 404.tsx file to the root of the pages folder and default export the desired component. 

## General Layout

The_app file renders the entire application. This means that in this file, you can import the global css as well as any components that you want to appear in the entire application. 

For example, to add a header to the entire application:

 1. create a Header component:![screenshot-gluki udemy com-2023 07 31-20_34_57](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/e2fa3072-95d3-43ef-a286-189e794d8ea9)

    
 2. create a Layout component
![screenshot-gluki udemy com-2023 07 31-20_36_20](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/d38431f4-759f-43f9-a7be-e2c1779dfd2a)
    
 3. in _app, wrap the Component file in the Layout comp
![screenshot-gluki udemy com-2023 07 31-20_37_15](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/11dd0045-c29d-42dd-b41d-fdbea14bcd9f)

## Sort-by Pages

If you want to generate a page which is based on some sorting criteria, one way to do this in Next is to set the URL of the page based on the selected criteria (in a drop down menu for example) and navigating to that page upon submission. Once navigated to that page [...slug] page will be rendered and that page can be set to fetch and display data based on the URL fragments. 

 - create the sort by menu component which should trigger a hander on submission
   
 - the![screenshot-gluki udemy com-2023 08 01-13_01_08](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/a9183a98-4945-4138-bb7b-222a23367f65)
   
- handler should prevent default behaviour as well as keep track of the values that are being submitted. This can be done using the useRef hook if working in the client.
  
![screenshot-gluki udemy com-2023 08 01-13_01_46](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/9318eadd-e988-4c07-8349-f63e4fd662e8)

 - the hander should then invoke sorting function with the values. This function comes as a prop from the parent component which is a page where the sorting menu is rendered.
 - in the parent component the sorting function should use the values it is invoked with to navigate to a new page with the url matching the values passed.

![screenshot-gluki udemy com-2023 08 01-13_04_47](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/9ca1762a-bfba-43c1-8dd9-1258fc0813c0)


 - the [...slug] page should then grab the URL values - using useRounter query if on the client - to fetch data based on the invoked values and then render the same on the page. 
 - remember that the useRouter [...page] will always return an array with all the queries not an key/value pair like in all other pages.

>  note that in this method, this component will be rendered twice. The first time it will be rendered without the url and only upon the second time will the URL be available so need to take account of that when fetching any data.
>  Also always need to handle errors for this type of scenario. For example, if the fetching method expects certain values like numbers, you's only want to invoke the function if the correct values have been entered in the URL. An example of error handler could look like this:
> 
> ![screenshot-gluki udemy com-2023 08 01-13_14_58](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/0fc02c84-849a-46d6-9f11-adad10055ef4)
> 
> You also want to handle a situation of what happens when the fetch does not return any data:
> 
> ![screenshot-gluki udemy com-2023 08 01-13_17_22](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/0d32c109-9fff-4505-84ad-fbfafde67f3a)


### Static Generation vs Incremental Static Generation vs Server Side Rendering vs Server Side Components

 - SG happens when you pre-render the HTML/DOM and all of the data that is required on the load, on the server in advance/during **built time and before deployment** 
	 - So all pages which are built to be statically generated/pre-built in advance at load time and remain of the same content unless you include Incremental Static Generation or run the build again with updated content. 
	 - Because SG pages are pre-generated at build time, once deployed, they can be cached by server/CDN serving the app hence requests can be served instantly. 
	 - Once these pages are served, they are hydrated with JS/React to make them interactive. 
	 - You can tell Next that you want your pages to be pre-generated by exporting certain functions from the Pages directory in Next.js app - `export async function getStaticProps(context){ ...}` - in there you can run any code that you would normally run on the server side only, any dependencies not included in the client bundle, any credentials never make it to the client, you can use node objects and access the system to read files etc. 
 - ISG is like SG but it updates at set intervals
 - SSR is rendering pages on the server upon http request 
 - SSC are react components that only ever run on the server. They can include ready-made SSC like getStaticProps and custom components. By default, all components inside App directory are server components. Server components can have client components as children but clinet components cannot have server components as children.

# Static Generation in Next.js

 - will happen **automatically** if you create/export a component inside of Pages folder the without dynamic data: 
 - will also happen automatically if you export getStaticProps from a Pages file

## Get Static Props

 - executed once at built time
 - during the built process, it runs before the component it serves;
 - only ever runs on the server unlike the component it serves which will be part of the client code;
 - any imports used only within getStaticProps will be stripped out of the client bundle/allows for easy code splitting
 - allows you to access libraries/Node objects that you use on the server like fs or process;
 - when the getStaticProps is executed by Next, Next will always assume that the file sits in the root of the directory and not in the Pages folder;
 - once built, the HTML will contain a script tag that will contain all of the data passed between the getStaticProps and the served component; this is necessary to ensure that during the hydration process, React knows what data the pre-generated component was filled with so it can then work with that data.

   ![screenshot-gluki udemy com-2023 08 02-11_31_19](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/439b7805-a075-4586-8ab2-720e0a2403ed)

 ### notFound key
 - accepts a boolean 
 - if set to true, will render 404 page - why would you want that? It allows you to return an error page if for whatever reason the data you are fetching inside of the getStaticProps does not return the expected data.
   
   ![screenshot-gluki udemy com-2023 08 02-13_49_17](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/296aa1ef-295f-4e2b-a830-1e12474a1228)

### redirect key

 - allows you to redirect the user to another page rather than showing the content of the page
   
 ![screenshot-gluki udemy com-2023 08 02-13_53_19](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/8ba44218-41d4-4af2-ab3b-45913ce19d6b)

## Build Process 

 - executing the build script will build the 'optimized production build'
 - once the command is run, Next will user side capabilities in the following ways: 

![screenshot-gluki udemy com-2023 08 02-11_36_19](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/22a11bd3-b37c-447f-a743-5b39dafe9833)

 - Static refers to any components that have hard coded return/do not use props/any data - pre-generated at built time - 404 page for example. 
 - SSG - static site generation is when you use getStaticProps and pass the same to the necessary component - pre-generated at built time
 - ISR - Incremental Static generation is when you use getStaticProps with the validate key - pre-generated at built time + re-validated at the set intervals 
 - SS - Server side is when the page is generated at run time at the time the http request is sent. 

 - You can further see the output of the build command in the .next folder. In the server subfolder you should have all of the html files which have been generated at build time.  
 - npm start - This will start the production ready page with a node.js server locally.

## Hosting implications
- none as does not run on the server which serves the application but rather a local node.js sever (created by Next) which builds the app on your machine when you execute npm run build. The static files are then hosted so no special node.js server hosting capability is required. 

 # Incremental Static Generation 

 - ensures that you do not just generate your page once statically at built time but it is continuously updated even after deployment without you re-deploying it. 
 - allows you to re-generate the page at every incoming request but at most by every 60 seconds (for example).
 - The page is re-created at most every 60 seconds.
 - If there are no requests for 3 days, the page will not be re-created every 60 seconds during those three days. The idea is that upon request, there is a capability to check how long ago the page was last generated and if it less than 60 seconds ago then the current page will be served otherwise a new page is generated on the server and sent to the clinet.
 - NOTE THAT IN THE DEVELOPMENT SERVER, YOU ALWAYS SEE THE LATEST VERSION OF THE PAGE AS THE PAGE IS ALWAYS RE-GENERATED WHEN YOU UPDATE YOUR FILES. IN PRODUCTION HOWEVER, IT WILL ONLYBE RE-GENERATED BASED ON WHAT INYERVALS YOU HAVE SET - However you can test production locally by running npm start which will start the local server sercing the app. This should behave like real production site and should only generate as per the intervals set which you can test with for exmaple, console.logs.

> 1. request has been made for a page. The page has last been generated 30 seconds ago. Current version is sent by the server.
> 2.  request has been made. The page has last been generated 65 seconds ago. A new page is generated by the server and cached and sent to the client replacing the old version.
> ![screenshot-gluki udemy com-2023 08 02-13_23_21](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/0261350f-ba47-4aa0-86c9-cfb528a7a9a6)

 - Alternative to using this method would be to use Static Generation for the initial load and then on the client, use useEffect to fetch any updates to the page from the server;

## Syntax 

![screenshot-gluki udemy com-2023 08 02-13_37_47](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/c28d11a0-a363-4bc7-84f0-cba5e408256f)

YOU CAN ALWAYS CHECK WHETHER NEXT HAS PICKED UP THE KEY BY RUNNING THE BUILD AND MAKING SURE THAT THE OUTPUT IN THE TERMINAL REGISTERED THAT ISG HAS BEEN INTRODUCED FOR THAT PAGE AND THEREFORE THE PAGE WILL BE RE-GENERATED AS PER THE INTERVAL SPECIFIED.

#  getStaticPaths

- in dynamic pages - pages like [id] for example - the default behaviour is that these pages are not pre-generated in advance. Instead, they are generated **at run time when a request for the page is made**. In other words: you make http req for domain.com/products/173664 and Next can use this request to generate the page on the server using the concrete value of "173664"/fetch any data for that page and send it back. 
- This is because of their dynamic nature Next does not know what concrete value will replace [id] until the request is made. Next also does not know how many pages will need to be generated in advance and the default behaviour is such, that it does not need to know this. Therefore if you are fetching data in the regular react component or using serverSideProps, the default behaviour is preserved and each [id] page is generated at runtime. Even if you are not using any dynamic data, the default behaviour will persist and the page will be generated at run time.  - YOU CAN CONFIRM THIS BY RUNNING NPM BUILD
- this is unlike non-dynamic URL - such as index.js in the root which has a url of domain.com - which will always be pre-generated by default. Any page including getStaticProps as well as any page without any dynamic data that does not sit inside of a url including [dynamic] value, will also always be pre-generated by default. 
- if you want to pre-generate [id] page by adding getStaticProps, you will need to let Next.js know how many pages you need and which concrete values you need in advance so that these can be **pre-generated at build time**. This is done using the getStaticPaths function. YOU CAN CONFIRM IF ALL THE PAGES YOU HAVE SPECIFIED TO BE PRE-GENERATED HAVE BEEN PRE-GENERATED BY RUNNING NPM BUILD

![screenshot-gluki udemy com-2023 08 03-16_46_29](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/15e71f97-eeee-4ab6-8d6b-0d1a78e9148b)

GENERATED DYNAMICALLY:

![screenshot-gluki udemy com-2023 08 03-17_05_52](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/cf8b7e50-675a-4c11-8f6f-e1c00dd60832)

![screenshot-gluki udemy com-2023 08 03-17_06_35](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/8a65f1ef-c4d7-41b9-b88e-04e50ba44201)

> *Pages Next pre-generates by default:*
	(1) pages with react components that do not use props / return hard coded data; 
	(2) pages which use props/fetch data but that data is fetched in getStaticProps
	
> *Pages Next generates at run time by default:*
	(1) any page which includes [dynamic] values in their URL
	(2) pages that use getServerSideProps etc
	
	
### How does generation with dynamic values work?

 - Any pages included in the getStaticPath will have their data fetched as soon as the application is loaded (before you even navigate to the dynamic URL) - so if you have 5 pages pre-generated, 5 separate json files will be fetched and each relevant file is used to hydrate the page in advance of it being visited. 
 - this means that once you navigate to the dynamic URL, the page will be loaded like in a normal React app, but no http request will be sent to the server for that page. Instead, the page will be hydrated using the pre-fetched data. 

### fallback key
- allows you to pre-render some pages - not all of them
- allows you to pre-generate highly visited pages and postpone generation of less visited pages until they are needed.
- for example, if you are running a blog, there is no point pre-generating blog pages that are never read as that's a waste of resources. Or on e-commerce website with millions of products, it would be very time consuming to pre-generate pages for all of the products. 
- if you do not want to pre-generate all pages, set fallback to true, otherwise it should be false. 
- when the fallback is set to true and you visit a page that is not included on the getStaticPaths, then it will be generated in accordance with the default behavior i.e. at runtime IF THE GIVEN DATA THAT IS REQUESED EXISTS.
- IF THE DATA DOES NOT EXIST you need to add an in statement checking if the requested data exists and if not return notFound: true inside of getStaticProps becuase that is where you are fetching the data on request and are able to check whether the requested info exists. 

> IT IS IMPORTANT THAT FOR THOSE PAGES THAT ARE GENERATED AT LOAD TIME,
> YOU ADD A LOADING STATE TO YOUR REACT COMPONENT AS THE DATA FOR THAT
> PAGE WILL NOT BE AVALIABLE IMMIDIETLY AND THEREFORE LOADING STATE IS
> REQUIRED. THIS IS UNLESS YOU SET THE FALLBACK TO 'blocking' WHICH WILL BLOCK THE PAGE FROM DISPLAYING UNTIL IT IS READY TO BE SHOWN TO THE USER

this is therefore a little bit like fetching on the client side but you dont actually have to fetch. Instead, you check whether what is being requested is in the props and if it is not, you show the loading state and once it is there, next will update the component and show the data. ---> *for those pages that are not pre-generated, does getStaticProps effectively act like getServerSideProps???? Becuase if the data is not there in the props, how does Next know where to fetch the data from? It must run getStaticProps code to get it hence getStaticProps is triggered at a time other than the build time/validation?*

# Server Side Rendering

 - useful when you want to pre-render a page on the server on *every request* and you also need access to the request itself which is not possible in pre-generation; 
 - achieved in Next using serverSideProps(); 
 - example of usage would be generating a user profile page:
	 - when the user requests the page, the data would be generated based on which user has made the request 
	 - you cannot pre-generate this page in advance as you do not want to use [dynamic] pages for this kind of information as you don't want everyone to be able to type in the URL and see profile page of the user 
	 - getServerSide props will allow you to identify the user that has sent the request because it has access to the request 
 - syntax: returns the same object as getStaticProps but for the invalidate key which is not needed.

![screenshot-gluki udemy com-2023 08 04-11_14_40](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/113eb747-371f-4369-a44d-0390d2f924c9)

## getStaticProps in dynamic pages 

 - you just use getServerSide props and extract the params in the same way from the context prop except you do not need getStaticPaths because this runs on the server upon request and is not pre-generated.

## getStaticProps in dynamic pages 

 - you just use getServerSide props and extract the params in the same way from the context prop except you do not need getStaticPaths because this runs on the server upon request and is not pre-generated. 

# Client side data fetching

 - some data does not need to be pre-rendered 
	 -  data that changes very frequently - the pre-rendered data sent from the server would always be outdates if the data itself changes very frequently. 
	 - pages where a quick navigation to a page might be more important than having the data avaliable as soon as the page is loaded. 
	 - when you create a page and use client side data fetching, the page will still be pre-generated by Next however in the HTML, you will only have the jsx of the initial load before useEffect has been triggered, for example, a loader. Therefore the data used by the page will not be pre-generated by Next but the page itself will. 

## SWR hook

 - created by Next.js team and requires installing a package 
 - allows data fetching using the fetch API by default, however adds extra features like caching. 
 - you can destruct the data and error objects from the hook and use the same to render what you need if there is an error or if there is no data. 
 - The data returned from the hook might need parsing and to do that you might define a fetcher function which will control how the data is retuned from the hook. See the docs.

## combining pre-fetching with client side fetching 

 - allows you to pre-fetch/generate a page with some data and then update that data on the client side if necessary; 
 - this means that you will use getStaticProps to fetch the data at built time and possibility add re-validate key, and then pass that data to the react component. In the react component, you can use the props to initialise data state. The state will obvs be overwritten as soon as useEffect is triggered on the second render which is there the data will be updated but importantly, on the first render you will have useful data to render and include in the source page.
 - any data that is fetched on the client will not be visible in the source page and this is why you need re-validate key as you want to make sure that the page source is as much up-to-date as possible.

	    import { useEffect, useState } from 'react';
	    import useSWR from 'swr';
	    
	    function LastSalesPage(props) {
	      const [sales, setSales] = useState(props.sales);
	      // const [isLoading, setIsLoading] = useState(false);
	    
	      const { data, error } = useSWR(
	        'https://nextjs-course-c81cc-default-rtdb.firebaseio.com/sales.json',
	        (url) => fetch(url).then(res => res.json())
	      );
	    
	      useEffect(() => {
	        if (data) {
	          const transformedSales = [];
	    
	          for (const key in data) {
	            transformedSales.push({
	              id: key,
	              username: data[key].username,
	              volume: data[key].volume,
	            });
	          }
	    
	          setSales(transformedSales);
	        }
	      }, [data]);
	    
	      // useEffect(() => {
	      //   setIsLoading(true);
	      //   fetch('https://nextjs-course-c81cc-default-rtdb.firebaseio.com/sales.json')
	      //     .then((response) => response.json())
	      //     .then((data) => {
	      //       const transformedSales = [];
	    
	      //       for (const key in data) {
	      //         transformedSales.push({
	      //           id: key,
	      //           username: data[key].username,
	      //           volume: data[key].volume,
	      //         });
	      //       }
	    
	      //       setSales(transformedSales);
	      //       setIsLoading(false);
	      //     });
	      // }, []);
	    
	      if (error) {
	        return <p>Failed to load.</p>;
	      }
	    
	      if (!data && !sales) {
	        return <p>Loading...</p>;
	      }
	    
	      return (
	        <ul>
	          {sales.map((sale) => (
	            <li key={sale.id}>
	              {sale.username} - ${sale.volume}
	            </li>
	          ))}
	        </ul>
	      );
	    }
	    
	    export async function getStaticProps() {
	      const response = await fetch(
	        'https://nextjs-course-c81cc-default-rtdb.firebaseio.com/sales.json'
	      );
	      const data = await response.json();
	    
	      const transformedSales = [];
	    
	      for (const key in data) {
	        transformedSales.push({
	          id: key,
	          username: data[key].username,
	          volume: data[key].volume,
	        });
	      }
	    
	      return { props: { sales: transformedSales } };
	    }
	    
	    export default LastSalesPage;

# Head Component

 - can be added anywhere in the jsx code, does not have to be added at the top level
 - stores any html tags that are found in head like title and meta
 - the content you add to meta with name of description is the content found in the search results


![screenshot-gluki udemy com-2023 08 07-15_08_17](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/58785e79-0afb-40b0-8354-0cab925e9125)

## dynamic head content 

 - it is useful to use the actual data being used by the component in the Head to ensure the metadata is specific to the pgae

![screenshot-gluki udemy com-2023 08 07-15_10_37](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/35fcffca-6a23-43d4-b84a-d4f29cce0f37)


### re-using Head logic 
- you can put the Head code into a variable and then use the variable in every instance of the page, for example, all if statements returning errors/messages as well as the primary return statement.

  ![screenshot-gluki udemy com-2023 08 07-15_14_32](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/09b3e2ea-bb96-4acf-89fd-6bfc160aa184)


## _app.jsx - app wide settings inside of the app component tree

 - lives in pages folder at root level
 - root app component inside rendered for every single page that is being displayed. 
 - allows you to add application wide settings
 - this means that anything that you include in that file will be displayed on each page.
 - any head data that you would want to be added to all pages can be added here
 - if you have the head element set up here as well as on individual pages, Next will merge these together - if there is a conflict, the latest tag wins. 
 - you can set up general meta/title data in _app.js and then override them if necessary in each page.

![screenshot-gluki udemy com-2023 08 07-15_22_40](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/b6e9f49d-6846-4d54-8928-457dcead7f23)


## _document.js - app wide settings outside of the app component tree

 - not there by default but can be added manually 
 - has to be added in the pages folder at root level
 - allows to customise all elements that make up the HTML
 - requires special class component which extends the Document component from Next/Document.
 - Also requires special JSX which needs to be imported from the same
 - has to follow a special structure as below:

![screenshot-gluki udemy com-2023 08 07-15_37_02](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/4fe9584a-690c-4a12-a101-a612338fb5c1)


- allows you to add attributes to HTML tag or add divs outside of the app component tree - so that would be the same level as the id=root div in a normal rect app.

![screenshot-gluki udemy com-2023 08 07-15_48_53](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/c587214e-9dbb-4712-83b5-6e4ae6ed6129)

# Application wide state in Next.js

 - local state = useState - localised to each componenet 
 - application wide state - useContext - applies to the entire app - allows you to trigger components from other components 

## Set up context

### create context 

 - this creates the context data that will be shared using the createContext hook and initialises the same.

![screenshot-gluki udemy com-2023 08 08-14_46_27](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/85e864a2-f881-4d33-aa22-212c5cb7b2c6)


### create provider 

 - create context allows you to create provider component.

![screenshot-gluki udemy com-2023 08 08-14_49_58](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/7e182822-a38e-4cbf-8628-ada71c3e2f07)

 - the provider component is used to wrap any components which are will have access to the context. In Next, you will want to apply this in the _app.js file


 - the provider should also manage all the context related state:
	- set up new state to keep track of active notification within the context
	- create the functions which will handle logic for the functions in the context
	  
![screenshot-gluki udemy com-2023 08 08-15_02_54](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/aa41d683-36eb-4537-8486-036645db4b29)

 - the provider must return the context object which will be passed as a value in the provider component ie. it will be accessible in all components exposed to it

![screenshot-gluki udemy com-2023 08 08-15_09_47](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/27ebeb17-557b-4e7a-824f-ade1e2b11082)


