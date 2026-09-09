# Online Voting System

A PHP and MySQL online voting system for managing elections, voters, positions, candidates, and ballots.

## Features

### Admin dashboard

The dashboard provides an overview of the election:

- Total number of positions
- Total number of candidates
- Total number of registered voters
- Number of voters who have submitted ballots
- Vote totals displayed as charts for each position
- Printable vote-tally view where supported by the installation

### Election positions

Administrators can create, edit, and delete positions such as President or Secretary. Each position includes:

- A description or title
- A maximum number of candidates a voter may select
- A display priority that controls the order shown on the ballot and dashboard

### Candidate management

Administrators can add, edit, and delete candidates and assign each candidate to a position. Candidate records support:

- First and last name
- Position assignment
- Profile photo
- Campaign platform or description

The platform can be opened from the candidate list for review before voting.

### Voter management

Administrators can register, edit, and delete voters. The voter management screen supports:

- First and last name
- A separate password for every voter
- A generated unique 15-character voter ID
- An optional profile photo
- Updating voter photos after registration

Passwords are hashed with PHP's `password_hash()` function. The plain password is never stored in the database.

### Voter authentication

Voters sign in with their generated voter ID and personal password. Successful authentication creates a session and takes the voter to the ballot page. Invalid IDs and passwords show an error message without creating a voter session.

### Online ballot

The voter ballot displays the configured positions and their assigned candidates. The selection rules use each position's maximum-vote setting. Before submission, voters can review their selections through the preview screen.

### Vote recording

When a ballot is submitted, the selected candidates are saved in the `votes` table and associated with the voter's database ID, position, and candidate. The admin dashboard and votes page use these records to calculate and display results.

### Vote review and reset

Administrators can review recorded votes with the related position, candidate, and voter names. The admin votes screen also provides a reset action for clearing election results before a new election or test run.

### Admin profile and election configuration

The admin area includes controls for updating the administrator profile and changing the election title used by the application. The current election title is stored in [`admin/config.ini`](admin/config.ini).

## Requirements

- XAMPP with Apache and MySQL
- PHP 7.4 or later
- A web browser

## Installation

1. Copy the project folder to:

   ```text
   C:\xampp\htdocs\votesystem
   ```

2. Start **Apache** and **MySQL** from the XAMPP Control Panel.

3. Open phpMyAdmin:

   ```text
   http://localhost/phpmyadmin/
   ```

4. Create a database named `votesystem`.

5. Import [`db/votesystem.sql`](db/votesystem.sql) into the database.

6. Open the application:

   ```text
   http://localhost/votesystem/
   ```

## Database Configuration

The default connection is configured in [`includes/conn.php`](includes/conn.php):

- Host: `localhost`
- Username: `root`
- Password: empty
- Database: `votesystem`

Update that file if your MySQL installation uses different credentials.

## Admin Login

Open the admin login page:

```text
http://localhost/votesystem/admin/
```

For the database dump, the default account is:

- Username: `crce`
- Password: `password`

Change the password before using this system outside a local development environment.

## Creating Voters

1. Sign in to the admin panel.
2. Open **Voters**.
3. Select **Add New**.
4. Enter the voter's first name, last name, and password.
5. Save the voter.
6. Copy the generated voter ID.

Each voter receives a unique 15-character voter ID. Passwords are stored as secure password hashes and can be different for every voter.

## Voter Login

Open:

```text
http://localhost/votesystem/
```

Sign in with the generated voter ID and the password selected when the voter was created.

## Typical Workflow

1. Import the database and sign in as the administrator.
2. Configure the election title.
3. Create the election positions and set their maximum vote values.
4. Add candidates and assign them to positions.
5. Add voters and give each voter their generated ID and personal password securely.
6. Have voters sign in and submit their ballots.
7. Review the vote totals from the admin dashboard or votes page.
8. Print or reset the results when appropriate.

## Important Behavior

- A voter ID identifies the voter; it is not the same as the internal numeric database ID.
- Every voter can have a different password. Passwords do not need to be unique across voters.
- Vote records are linked to the voter's internal database ID, not the displayed voter ID.
- Deleting or resetting data should be done carefully because these actions may remove election records.
- The project currently uses direct SQL query strings in several files and is best treated as a local development project until the security items below are addressed.

## Main URLs

- Voter login: `http://localhost/votesystem/`
- Admin login: `http://localhost/votesystem/admin/`
- Admin dashboard: `http://localhost/votesystem/admin/home.php`

## Project Structure

```text
votesystem/
├── admin/          Admin pages and management actions
├── db/             Database schema and seed data
├── images/         Uploaded voter and candidate images
├── includes/       Shared voter-side PHP components
├── plugins/        Frontend JavaScript and CSS plugins
├── index.php       Voter login page
├── home.php        Voter ballot page
└── submit_ballot.php  Ballot submission handler
```

## Security Notes

This project is intended for local development and learning. Before production use:

- Use prepared statements instead of interpolating request values into SQL.
- Use a non-root MySQL account with a strong password.
- Add CSRF protection to forms.
- Validate and restrict uploaded files.
- Change the default admin password.
- Use HTTPS and secure session settings.



sunset00x ( github & linkedin)