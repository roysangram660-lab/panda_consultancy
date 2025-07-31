# panda_consultancy
Your trusted consulting partner for your polycables manufacturing unit .
panda_consultancy/
├── backend/
│   ├── server.js
│   ├── models/
│   ├── routes/
│   └── ...
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Navbar.js
│   │   │   ├── Footer.js
│   │   ├── pages/
│   │   │   ├── Home.js
│   │   │   ├── About.js
│   │   │   ├── Services.js
│   │   │   ├── Contact.js
│   │   │   ├── ApplyJob.js
│   │   │   ├── Companies.js
│   │   │   ├── Blog.js
│   │   ├── App.js
│   │   └── index.js
│   └── package.json
└── README.md
npx create-react-app .
npm install @mui/material @emotion/react @emotion/styled react-router-dom
import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Navbar from './components/Navbar';
import Footer from './components/Footer';
import Home from './pages/Home';
import About from './pages/About';
import Services from './pages/Services';
import Contact from './pages/Contact';
import ApplyJob from './pages/ApplyJob';
import Companies from './pages/Companies';
import Blog from './pages/Blog';

function App() {
  return (
    <Router>
      <Navbar />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/services" element={<Services />} />
        <Route path="/contact" element={<Contact />} />
        <Route path="/apply-job" element={<ApplyJob />} />
        <Route path="/companies" element={<Companies />} />
        <Route path="/blog" element={<Blog />} />
      </Routes>
      <Footer />
    </Router>
  );
}

export default App;
import React from 'react';
import { AppBar, Toolbar, Typography, Button, Box } from '@mui/material';
import { Link } from 'react-router-dom';

function Navbar() {
  return (
    <AppBar position="static" color="primary">
      <Toolbar>
        <Typography variant="h6" sx={{ flexGrow: 1 }}>
          Panda Consultancy
        </Typography>
        <Box>
          <Button color="inherit" component={Link} to="/">Home</Button>
          <Button color="inherit" component={Link} to="/about">About</Button>
          <Button color="inherit" component={Link} to="/services">Services</Button>
          <Button color="inherit" component={Link} to="/apply-job">Apply for a Job</Button>
          <Button color="inherit" component={Link} to="/companies">Companies</Button>
          <Button color="inherit" component={Link} to="/blog">Blog</Button>
          <Button color="inherit" component={Link} to="/contact">Contact</Button>
        </Box>
      </Toolbar>
    </AppBar>
  );
}

export default Navbar;
import React from 'react';
import { Box, Typography } from '@mui/material';

function Footer() {
  return (
    <Box component="footer" sx={{ p: 2, background: '#1976d2', color: '#fff', textAlign: 'center' }}>
      <Typography variant="body2">
        &copy; {new Date().getFullYear()} Panda Consultancy. All rights reserved.
      </Typography>
    </Box>
  );
}

export default Footer;
import React from 'react';
import { Container, Typography, Button, Grid } from '@mui/material';
import { Link } from 'react-router-dom';

function Home() {
  return (
    <Container sx={{ mt: 5, mb: 5 }}>
      <Typography variant="h3" gutterBottom>
        Welcome to Panda Consultancy
      </Typography>
      <Typography variant="h6" gutterBottom>
        Your trusted consulting partner for your polycables and plastics manufacturing unit.
      </Typography>
      <Typography variant="body1" sx={{ mb: 4 }}>
        We connect job seekers with top companies in the plastic industry and provide end-to-end consultancy for machinery, raw materials, and project tenders.
      </Typography>
      <Grid container spacing={2}>
        <Grid item>
          <Button variant="contained" color="secondary" component={Link} to="/apply-job">
            Apply for a Job
          </Button>
        </Grid>
        <Grid item>
          <Button variant="outlined" color="secondary" component={Link} to="/companies">
            Companies: Post a Tender
          </Button>
        </Grid>
      </Grid>
    </Container>
  );
}

export default Home;
backend/
├── models/
│   ├── JobApplication.js
│   ├── CompanyTender.js
├── routes/
│   ├── jobs.js
│   ├── tenders.js
├── server.js
├── package.json
npm init -y
npm install express mongoose cors
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');

const jobsRoute = require('./routes/jobs');
const tendersRoute = require('./routes/tenders');

const app = express();
app.use(cors());
app.use(express.json());

mongoose.connect('mongodb://localhost:27017/panda_consultancy', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

app.use('/api/jobs', jobsRoute);
app.use('/api/tenders', tendersRoute);

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log('Server running on port', PORT);
});
const mongoose = require('mongoose');

const JobApplicationSchema = new mongoose.Schema({
  name: String,
  email: String,
  phone: String,
  resume: String, // store url or base64 (for MVP, use url)
  appliedAt: { type: Date, default: Date.now },
});

module.exports = mongoose.model('JobApplication', JobApplicationSchema);
const mongoose = require('mongoose');

const CompanyTenderSchema = new mongoose.Schema({
  companyName: String,
  contactPerson: String,
  email: String,
  phone: String,
  projectDetails: String,
  submittedAt: { type: Date, default: Date.now },
});

module.exports = mongoose.model('CompanyTender', CompanyTenderSchema);

const express = require('express');
const router = express.Router();
const JobApplication = require('../models/JobApplication');

// Submit job application
router.post('/', async (req, res) => {
  try {
    const job = new JobApplication(req.body);
    await job.save();
    res.status(201).json({ message: 'Application submitted!' });
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});

// Get all applications (admin use)
router.get('/', async (req, res) => {
  const jobs = await JobApplication.find();
  res.json(jobs);
});

module.exports = router;
import React from 'react';
import { Container, Typography } from '@mui/material';

function About() {
  return (
    <Container sx={{ mt: 5, mb: 5 }}>
      <Typography variant="h4" gutterBottom>About Us</Typography>
      <Typography>
        Panda Consultancy is your trusted partner in the polycables and plastics industry, connecting talent and companies for a better tomorrow.
      </Typography>
    </Container>
  );
}

export default About;
import React from 'react';
import { Container, Typography, List, ListItem, ListItemText } from '@mui/material';

function Services() {
  return (
    <Container sx={{ mt: 5, mb: 5 }}>
      <Typography variant="h4" gutterBottom>Our Services</Typography>
      <List>
        <ListItem><ListItemText primary="Job placement for plastic/polycables sector" /></ListItem>
        <ListItem><ListItemText primary="Consultancy for machinery and raw materials" /></ListItem>
        <ListItem><ListItemText primary="Company tender submission & project management" /></ListItem>
        <ListItem><ListItemText primary="Industry insights: blogs, videos, and more" /></ListItem>
      </List>
    </Container>
  );
}

export default Services;
import React from 'react';
import { Container, Typography, TextField, Button, Box } from '@mui/material';

function Contact() {
  return (
    <Container sx={{ mt: 5, mb: 5 }}>
      <Typography variant="h4" gutterBottom>Contact Us</Typography>
      <Box component="form" sx={{ mt: 2 }}>
        <TextField label="Name" fullWidth sx={{ mb: 2 }} />
        <TextField label="Email" fullWidth sx={{ mb: 2 }} />
        <TextField label="Message" fullWidth multiline rows={4} sx={{ mb: 2 }} />
        <Button variant="contained" color="primary">Send</Button>
      </Box>
      <Typography sx={{ mt: 3 }}>
        Or reach us at: <strong>[Your Contact Number Here]</strong>
      </Typography>
    </Container>
  );
}

export default Contact;
import React, { useState } from 'react';
import { Container, Typography, TextField, Button, Box, Alert } from '@mui/material';
import axios from 'axios';

function ApplyJob() {
  const [form, setForm] = useState({ name: '', email: '', phone: '', resume: '' });
  const [success, setSuccess] = useState('');
  const [error, setError] = useState('');

  const handleChange = e => setForm({ ...form, [e.target.name]: e.target.value });

  const handleSubmit = async e => {
    e.preventDefault();
    try {
      await axios.post('http://localhost:5000/api/jobs', form);
      setSuccess('Application submitted!');
      setError('');
      setForm({ name: '', email: '', phone: '', resume: '' });
    } catch (err) {
      setError('Submission failed.');
      setSuccess('');
    }
  };

  return (
    <Container sx={{ mt: 5, mb: 5 }}>
      <Typography variant="h4" gutterBottom>Apply for a Job</Typography>
      <Box component="form" sx={{ mt: 2 }} onSubmit={handleSubmit}>
        <TextField label="Name" name="name" value={form.name} onChange={handleChange} required fullWidth sx={{ mb: 2 }} />
        <TextField label="Email" name="email" value={form.email} onChange={handleChange} required fullWidth sx={{ mb: 2 }} />
        <TextField label="Phone" name="phone" value={form.phone} onChange={handleChange} required fullWidth sx={{ mb: 2 }} />
        <TextField label="Resume (Link)" name="resume" value={form.resume} onChange={handleChange} required fullWidth sx={{ mb: 2 }} />
        <Button variant="contained" type="submit">Submit</Button>
      </Box>
      {success && <Alert severity="success" sx={{ mt: 2 }}>{success}</Alert>}
      {error && <Alert severity="error" sx={{ mt: 2 }}>{error}</Alert>}
    </Container>
  );
}

export default ApplyJob;
import React, { useState } from 'react';
import { Container, Typography, TextField, Button, Box, Alert } from '@mui/material';
import axios from 'axios';

function Companies() {
  const [form, setForm] = useState({ companyName: '', contactPerson: '', email: '', phone: '', projectDetails: '' });
  const [success, setSuccess] = useState('');
  const [error, setError] = useState('');

  const handleChange = e => setForm({ ...form, [e.target.name]: e.target.value });

  const handleSubmit = async e => {
    e.preventDefault();
    try {
      await axios.post('http://localhost:5000/api/tenders', form);
      setSuccess('Tender submitted!');
      setError('');
      setForm({ companyName: '', contactPerson: '', email: '', phone: '', projectDetails: '' });
    } catch (err) {
      setError('Submission failed.');
      setSuccess('');
    }
  };

  return (
    <Container sx={{ mt: 5, mb: 5 }}>
      <Typography variant="h4" gutterBottom>Company Tender Submission</Typography>
      <Box component="form" sx={{ mt: 2 }} onSubmit={handleSubmit}>
        <TextField label="Company Name" name="companyName" value={form.companyName} onChange={handleChange} required fullWidth sx={{ mb: 2 }} />
        <TextField label="Contact Person" name="contactPerson" value={form.contactPerson} onChange={handleChange} required fullWidth sx={{ mb: 2 }} />
        <TextField label="Email" name="email" value={form.email} onChange={handleChange} required fullWidth sx={{ mb: 2 }} />
        <TextField label="Phone" name="phone" value={form.phone} onChange={handleChange} required fullWidth sx={{ mb: 2 }} />
        <TextField label="Project Details" name="projectDetails" value={form.projectDetails} onChange={handleChange} required fullWidth multiline rows={4} sx={{ mb: 2 }} />
        <Button variant="contained" type="submit">Submit</Button>
      </Box>
      {success && <Alert severity="success" sx={{ mt: 2 }}>{success}</Alert>}
      {error && <Alert severity="error" sx={{ mt: 2 }}>{error}</Alert>}
    </Container>
  );
}

export default Companies;

















