"# Marketplace-Documentation-Hackathon-Bandage" 


Bandage - A Clothing and Household Items Website
Project Overview

Bandage is a web-based e-commerce platform offering a variety of clothing and household items. Built with modern web development tools, Bandage provides a seamless shopping experience. This document outlines the journey of building the project, including encountered problems, solutions, and steps for deployment.

Table of Contents
.Project Initialization
.Features
.Technologies Used
.Development Challenges and Solutions
.Key Hooks and State Management
.Error Handling
.Deployment
.Project Initialization

Setup:
.Initialized the project using create-next-app for a Next.js framework.
.Installed necessary dependencies such as tailwindcss, lucide-react, and sanity.
.Sanity Integration:
.Created a Sanity project for managing product data.
.Configured API routes to fetch data from Sanity into the website.

Features

.Dynamic Product Listing: Products fetched dynamically from Sanity CMS.
.Wishlist and Cart Management: Add, remove, and update items.
.Responsive Design: Optimized for both desktop and mobile users.
.Secure Checkout: Integrated payment processing (future implementation).

Technologies Used

.Frontend: Next.js, TailwindCSS
.Backend: Node.js, Sanity CMS
.Icons: Lucide React
.Deployment: Vercel

Development Challenges and Solutions

1. Data Fetching Issues from Sanity:
Problem: API requests were not returning expected data.
Solution:
Verified Sanity project ID and dataset configuration.
Used groq queries for structured data fetching.
Handled errors using try...catch in API calls.

2. Client-Side Rendering Issues:

Problem: Encountered useSearchParams() error due to improper Suspense boundary.
Solution:
Wrapped components using useSearchParams() in a Suspense boundary.
Ensured components are client-side rendered by adding the "use client" directive.

3. LocalStorage Synchronization:

Problem: Wishlist and cart items were not syncing properly.
Solution:
Managed state using useEffect to synchronize localStorage data.
Applied defensive coding to handle edge cases like empty or corrupted data.

4. Build and Deployment Issues:

Problem: Vercel builds failed due to server-side errors in fetching data.
Solution:
Switched to conditional data fetching for CSR.
Added error logging during the build process to debug missing environment variables.

Key Hooks and State Management

1.useState for Local State:

Used to manage cart and wishlist data.

Example:

const [wishlist, setWishlist] = useState<IProduct[]>([]);
useEffect for Side Effects:
.Synchronizing localStorage with the state.
.Fetching product data on component mount.
.useSearchParams for Query Parameters:
.Extracted product details from query strings for dynamic updates.
.Wrapped in a Suspense boundary to avoid runtime errors.

Error Handling

LocalStorage Handling:

.Ensured data validity with default fallbacks.
.Used try...catch blocks for JSON parsing errors.

API Errors:

.Logged API responses during development to catch issues early.
.Displayed user-friendly error messages for network failures.

Build-Time Errors:

Debugged Vercel build errors by inspecting logs.
Verified all required environment variables were set before deployment.

Deployment

Building the Project:

.Verified production readiness using next build.

Vercel Deployment:

.Pushed the project to GitHub and connected it to Vercel.

Configured environment variables for Sanity API in the Vercel dashboard.

Post-Deployment Testing:

.Verified data fetching, user interactions, and responsiveness.
.Fixed any runtime issues directly on Vercel.

Future Improvements

.Add server-side rendering (SSR) for SEO optimization.
.Integrate a payment gateway for a complete e-commerce experience.
.Implement better error tracking using monitoring tools like Sentry.



Fetching Data from Sanity
Setting Up Sanity API
Install Sanity CLI:

bash
Copy
Edit
npm install -g @sanity/cli
Initialize Sanity Studio:
Navigate to your project directory and run:

bash
Copy
Edit
sanity init
Follow the prompts to create a new project or link to an existing one.

Configure the API Token:

Go to your Sanity project dashboard.
Navigate to Settings > API and generate a read-only token.
Add the token to your environment variables in .env.local:
bash
Copy
Edit
SANITY_TOKEN=your_api_token_here
Installing Required Packages
Install the necessary dependencies for connecting to Sanity:

bash
Copy
Edit
npm install @sanity/client
Fetching Data in Next.js
Use the following example code to fetch data from your Sanity backend:

typescript
Copy
Edit
import { createClient } from '@sanity/client';

const client = createClient({
  projectId: 'your_project_id',
  dataset: 'production',
  apiVersion: '2023-01-01', // Use a specific date for stable API behavior
  useCdn: true, // Set to false to fetch the latest data (for dev environments)
  token: process.env.SANITY_TOKEN,
});

// Fetch products or items
export const fetchProducts = async () => {
  const query = `*[_type == "product"] {
    _id,
    name,
    price,
    description,
    image
  }`;

  try {
    const products = await client.fetch(query);
    return products;
  } catch (error) {
    console.error('Error fetching products:', error);
    return [];
  }
};
Example Integration in Your App
In a Next.js page or component:

typescript
Copy
Edit
import { useEffect, useState } from 'react';
import { fetchProducts } from '../utils/sanity';

export default function Products() {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    const getProducts = async () => {
      const data = await fetchProducts();
      setProducts(data);
    };
    getProducts();
  }, []);

  return (
    <div className="container">
      <h1>Products</h1>
      <ul>
        {products.map((product) => (
          <li key={product._id}>
            <h2>{product.name}</h2>
            <p>{product.description}</p>
            <p>${product.price}</p>
          </li>
        ))}
      </ul>
    </div>
  );
}
Debugging Issues with Sanity API
Invalid Token: Ensure your token has sufficient permissions (e.g., read access).
Project ID Not Found: Verify that the projectId in your client matches the one in your Sanity dashboard.
Data Not Appearing:
Check if data is published in Sanity Studio.
Run sanity dataset list to ensure the correct dataset is selected.
Console Errors: Use console.error() to debug issues during API calls.
I found errors on build time which i solved first and then push the repository.
but my cards fetching from api sanity didnot appear on vercel . So i resolve this issue by making by component server component.


for performance testing i use Lighthouse tool.
for components testing i use cypress tool.
for API testing i use Postman and thunder client both.
