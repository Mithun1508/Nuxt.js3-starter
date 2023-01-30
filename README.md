Nuxt.js 3 + Supabase starter for Typescript lovers
Nuxt Starter Kit is an opinionated boilerplate based off of Nuxt3(beta), with all the bells and whistles you would want ready, up and running when starting a Nuxt project to play and experiment with.

PRs welcome! License Follow @aftabbuddy

Out of the box you get all the essentials

Typescript as the language choice
Tailwind CSS for quick styling without getting out of your HTML
Daisy UI for pre-made TailwindCSS component classes
Tailwind UI for robust headless logic you can use for components like Dialog/Modal, Dropdown, List, etc.
FontSource for effortless custom font integration
Icons through Unplugin for thousands of icons as components that are available on-demand and universally
ESLint for static code analysis (added but it's currently failing due to #171) and
Prettier for code formatting
with Supabase support

Authentication System with Supabase GoTrue
User Profiles available on /profile as an example for Supabase PostgREST (CRUD API)
User Avatar which is Supbase Storage(AWS S3 backed effortless uploads) supported
and a bunch of pre-made, hand-rolled(easily replace-able) components, that you almost always end up installing/using for any non-trivial project

Button Button with DaisyUI style support for all the basic use cases
Alert/Toast to notify your users of the outcome of an event - success, errorordefault` is supported
Modal(feat. Headless UI) as you always come back to `em
Loaders for reporting the progress of an API call + a page load
Avatar for user avatar's
and more...

Note: Refer the basic branch for a bare minimum starter structure with all the essentials

🚧 Nuxt 3 is currently in beta and is not yet production ready. 🚧 Use const { $supabase } = useNuxtApp() to access Supabase client. Composables built around Supabase like useSupabase, although available are pretty much unusable due to initialization issues

Quick Start
The best way to start with this template is to click "Use this template" above, create your own copy and work with it

Development
To start the project locally, run:

yarn dev
which kickstarts the nuxt3 development and build server nuxi. Open http://localhost:3000 with your browser to see the result.

Check package.json for the full list of commands available at your disposal.

How to Setup Supabase for Nuxt Starter Kit?
If new to Supabase

Create account at Supabase
Create a Organisation, and a project
Once done, or if you already have a Supabase project

Copy the generated project's API authentication details from https://app.supabase.io/project/<your-awesome-nuxt-project>/api/default?page=auth
Place the details in .env as SUPABASE_URL and SUPABASE_KEY
Install NPM dependencies, by running yarn
Nuxt Start Kit supports user profiles and user avatars. To get the profile table and storage ready, execute the following queries at https://app.supabase.io/project/<your-awesome-nuxt-project>/editor/sql

-- Create a table for Public Profiles
create table profiles (
  id uuid references auth.users not null,
  username text unique,
  avatar_url text,
  website text,
  updated_at timestamp with time zone,

  primary key (id),
  unique(username),
  constraint username_length check (char_length(username) >= 3)
);

alter table profiles enable row level security;

create policy "Public profiles are viewable by everyone."
  on profiles for select
  using ( true );

create policy "Users can insert their own profile."
  on profiles for insert
  with check ( auth.uid() = id );

create policy "Users can update own profile."
  on profiles for update
  using ( auth.uid() = id );

-- Set up Storage!
insert into storage.buckets (id, name)
values ('avatars', 'avatars');

create policy "Avatar images are publicly accessible."
  on storage.objects for select
  using ( bucket_id = 'avatars' );

create policy "Anyone can upload an avatar."
  on storage.objects for insert
  with check ( bucket_id = 'avatars' );
Known Issues
ESLint - Once the issue is resolved you can add
        "*.+(js|ts|vue)": [
            "yarn run lint"
        ],
in package.json under the lint-staged section for linting on commits

License
MIT