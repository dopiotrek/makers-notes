# Basics
Claims are not on the user_metadata object NOT the raw_user_meta_data object, which can be seen in the auth.users table. 
The custom claims are only seen in the JWT (you need to encode it to see it; use jwt.io)

# Supabase Function
- note 1: change the email on line 35 to the email you want to use as an admin
- note 2: I use role id here, e.g. 1 or 2, as I have a table on public.roles that has the roles and their id's, which I then map to the text


declare
  claims jsonb;
  user_email text;
begin
  -- Get the user's email from the correct location in the event
  user_email := coalesce(
    event->>'email',                              -- Try direct email
    event->'raw_user_meta_data'->>'email',       -- Try raw metadata
    event->'claims'->'user_metadata'->>'email'    -- Try claims metadata
  );
  
  RAISE LOG 'START - Processing role for email: %', user_email;
  
  -- Get existing claims or initialize empty object
  claims := coalesce(event->'claims', '{}'::jsonb);
  RAISE LOG 'Current claims: %', claims;
  
  -- Initialize user_metadata if it doesn't exist
  if not claims ? 'user_metadata' then
    claims := jsonb_set(claims, '{user_metadata}', '{}'::jsonb);
    RAISE LOG 'Initialized user_metadata';
  end if;
  
  -- Add explicit email comparison logging
  RAISE LOG 'Comparing email % with your@email.com', user_email;
  
  -- Set roleId directly in user_metadata with null check
  if user_email is not null and lower(user_email) = lower('your@email.com') then
    claims := jsonb_set(claims, '{user_metadata,roleId}', '"1"');
    RAISE LOG 'Setting ADMIN role (1) for: %', user_email;
  else
    claims := jsonb_set(claims, '{user_metadata,roleId}', '"2"');
    RAISE LOG 'Setting USER role (2) for: %', user_email;
  end if;
  
  -- Log final claims before returning
  RAISE LOG 'END - Final claims: %', claims;
  RAISE LOG 'END - Final email used: %', user_email;
  
  -- Set the updated claims in the event
  return jsonb_set(event, '{claims}', claims);
end;
